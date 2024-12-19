# Testing de Composables con Vitest

En esta sección vamos a comprender mejor cómo probar un composable de **Vue**. Hoy en día, gran parte de nuestra lógica comercial o lógica de interfaz de usuario suele estar encapsulada en **composables**, por lo que creo que es importante comprender cómo probarlos.

## Definiciones

Primero, comprendamos algunos conceptos básicos sobre las pruebas. Esto ayudará a aclarar en qué lugar se ubican las pruebas de los **composables** en el panorama más amplio de las pruebas de software.

### Composables

Los **Composables** son funciones reutilizables que encapsulan y administran estados y lógica reactiva. Permiten una forma flexible de organizar y reutilizar el código en los componentes, lo que mejora la modularidad y facilidad de mantenimiento.

### Pirámide de prueba

La **pirámide de pruebas** es una metáfora conceptual que ilustra el equilibrio ideal entre los distintos tipos de pruebas. Recomienda una gran base de pruebas unitarias, complementada con un conjunto más pequeño de pruebas de integración y rematada con un conjunto aún más pequeño de pruebas de extremo a extremo. Esta estructura garantiza una cobertura de pruebas eficiente y eficaz.

### Pruebas unitarias en los composables

Las **pruebas unitarias** se refieren a la práctica de probar unidades individuales de código de forma aislada. En el contexto de Vue, probar un composable es una forma de prueba unitaria. Implica verificar rigurosamente la funcionalidad de estos bloques de código aislados y reutilizables, asegurándose de que funcionen correctamente sin dependencias externas.

## Prueba de composables

Los composables son esencialmente funciones que aprovechan el sistema de reactividad de Vue. Podemos clasificar los composables en diferentes tipos:

### `Independent Composables`

Un **composable independiente** utiliza exclusivamente las API de reactividad de Vue. Estos composable funcionan independientemente de las instancias de componentes de Vue, lo que hace que sea fácil probarlos.

#### Ejemplo y estrategia de prueba

```typescript
import { Ref, computed, ComputedRef } from "vue";

function useSum(a: Ref<number>, b: Ref<number>): ComputedRef<number> {
  return computed(() => a.value + b.value);
}
```

Para probar este composable debemos invocarlo directamente y confirmar su estado devuelto.

```typescript
describe("useSum", () => {
  it("correctly computes the sum of two numbers", () => {
    const num1 = ref(2);
    const num2 = ref(3);
    const sum = useSum(num1, num2);

    expect(sum.value).toBe(5);
  });
});
```

### `Depedent Composables`

Los **Composables dependientes** se distinguen por su dependencia de la instancia del componente de Vue.A menudo, aprovechan características como los hooks de ciclo de vida o el contexto para su funcionamiento.

#### Ejemplo

Un ejemplo de **Composable dependiente** es `useLocalStorage`. Este facilita la interacción con el almacenamiento local del navegador y aprovecha el hook `onMounted` para la inicialización

```typescript
import { ref, computed, onMounted, watch } from "vue";

function useLocalStorage<T>(key: string, initialValue: T) {
  const value = ref<T>(initialValue);

  function loadFromLocalStorage() {
    const storedValue = localStorage.getItem(key);
    if (storedValue !== null) {
      value.value = JSON.parse(storedValue);
    }
  }

  onMounted(loadFromLocalStorage);

  watch(value, (newValue) => {
    localStorage.setItem(key, JSON.stringify(newValue));
  });

  return { value };
}

export default useLocalStorage;
```

#### Estrategia de prueba

Para realizar pruebas de manera eficaz en `useLocalStorage`, nos entrentamos a un desafío. Empecemos con una configuración básica:

```typescript
describe("useLocalStorage", () => {
  it("should load the initialValue", () => {
    const { value } = useLocalStorage("testKey", "initValue");
    expect(value.value).toBe("initValue");
  });

  it("should load from localStorage", async () => {
    localStorage.setItem("testKey", JSON.stringify("fromStorage"));
    const { value } = useLocalStorage("testKey", "initialValue");
    expect(value.value).toBe("fromStorage");
  });
});
```

En este caso, la primera prueba se aprobará y se afirmará que el composable se inicializa con el valor `initValue`. Sin embargo, la segunda prueba, que espera que el composable cargue un valor preexistente de localStorage, falla. Esto porque `onMounted` no se activa durante la prueba. Para solucionar esto, debemos refactorizar nuestro composable o nuestra configuración de prueba para simular el proceso de montaje del componente.

### Mejoras de las pruebas con `withSetup`

Para facilitar las pruebas de los composables que dependen de los hooks de ciclo de vida de Vue, desarrollamos una función de orden superior, `withSetup`. Esta utilidad nos permite crear un contexto de componente de manera programática.

#### Introducción a `withSetup`

`withSetup` está diseñado para simular la función de configuración de un componente de Vue, lo que nos permite probar composables en un entorno que imita de cerca su uso en el mundo real. La función acepta un composable y devuelve tanto el resultado del composable como una instancia de la aplicación Vue. Esta configuración permite realizar pruebas integrales, incluídas las funciones de ciclo de vida y reactividad.

```typescript
import type { App } from "vue";
import { createApp } from "vue";

export function withSetup<T>(composable: () => T): [T, App] {
  let result: T;
  const app = createApp({
    setup() {
      result = composable();
      return () => {};
    },
  });
  app.mount(document.createElement("div"));
  return [result, app];
}
```

`withSetup` se monta en una aplicación Vue mínima y se ejecuta el composable proporcionado durante la fase de configuración.

#### Utilizando `withSetup` en pruebas

Con `withSetup`, podemos mejorar nuestra estrategia de pruebas para composables como `useLocalStorage`, garantizando que se comporten como se espera incluso cuando dependen de los hooks de ciclo de vida:

```typescript
it("should load the value from localStorage if it was set before", async () => {
  localStorage.setItem("testKey", JSON.stringify("valueFromLocalStorage"));
  const [result] = withSetup(() => useLocalStorage("testKey", "testValue"));
  expect(result.value.value).toBe("valueFromLocalStorage");
});
```

Esta prueba permite que el composable se ejecute como si fuera parte de un componente normal, lo que garantiza que `onMounted` se active como se espera.

### Pruebas de composables con Inject

Otro escenario común es probar composables que dependen del sistema de inyección de dependencias de Vue mediante `inject`. Estos composables presentan desafíos únicos, ya que esperan que os componentes antecesores proporcionen ciertos valores.

#### Ejemplo de composable con Inject

```typescript
import type { InjectionKey } from "vue";
import { inject } from "vue";

export const MessageKey: InjectionKey<string> = Symbol("message");

export function useMessage() {
  const message = inject(MessageKey);

  if (!message) {
    throw new Error("Message must be provided");
  }

  const getUpperCase = () => message.toUpperCase();
  const getReversed = () => message.split("").reverse().join("");

  return {
    message,
    getUpperCase,
    getReversed,
  };
}
```

Para probar los componentes que usan inyección, necesitamos una función auxiliar que cree un entorno de prueba con los proveedores necesarios. Aquí hay una función de utilidad que lo hace posible:

```typescript
import type { InjectionKey } from "vue";
import { createApp, defineComponent, h, provide } from "vue";

type InstanceType<V> = V extends { new (...arg: any[]): infer X } ? X : never;
type VM<V> = InstanceType<V> & { unmount: () => void };

interface InjectionConfig {
  key: InjectionKey<any> | string;
  value: any;
}

export function useInjectedSetup<TResult>(
  setup: () => TResult,
  injections: InjectionConfig[] = []
): TResult & { unmount: () => void } {
  let result!: TResult;

  const Comp = defineComponent({
    setup() {
      result = setup();
      return () => h("div");
    },
  });

  const Provider = defineComponent({
    setup() {
      injections.forEach(({ key, value }) => {
        provide(key, value);
      });
      return () => h(Comp);
    },
  });

  const mounted = mount(Provider);

  return {
    ...result,
    unmount: mounted.unmount,
  } as TResult & { unmount: () => void };
}

function mount<V>(Comp: V) {
  const el = document.createElement("div");
  const app = createApp(Comp as any);
  const unmount = () => app.unmount();
  const comp = app.mount(el) as any as VM<V>;
  comp.unmount = unmount;
  return comp;
}
```

Con nuestra función auxiliar en su lugar, ahora podemos escribir pruebas integrales para nuestro composable dependiente de inyección:

```typescript
import { describe, expect, it } from "vitest";
import { useInjectedSetup } from "../helper";
import { MessageKey, useMessage } from "../useMessage";

describe("useMessage", () => {
  it("should handle injected message", () => {
    const wrapper = useInjectedSetup(
      () => useMessage(),
      [{ key: MessageKey, value: "hello world" }]
    );

    expect(wrapper.message).toBe("hello world");
    expect(wrapper.getUpperCase()).toBe("HELLO WORLD");
    expect(wrapper.getReversed()).toBe("dlrow olleh");

    wrapper.unmount();
  });

  it("should throw error when message is not provided", () => {
    expect(() => {
      useInjectedSetup(() => useMessage(), []);
    }).toThrow("Message must be provided");
  });
});
```

`useInjectSetup` crea un entorno de prueba que:

1. Simula una jerarquía de componentes.
2. Proporciona los vaores de inyección necesarios.
3. Ejecuta el composable en un contexto Vue adecuado.
4. Devuelve el resultado del componente junto con una función de desmontaje.

Este enfoque nos permite:

- Propar los composables que dependen de la inyección.
- Verificar el manejo de errores cuando faltan las inyecciones requeridas.
- Probar la funcionalidad completa de los métodos que usan los valores inyectados.
- Limpiar adecuadamente después de las pruebas desmontando el componente de prueba.

> Aclaración:
>
> Siempre desmontar el componente de prueba para evitar pérdida de memoria y garantizar el aislamiento de la prueba.

## Resumen

| Composables independientes 🔓                               | Composables dependientes ⛓️‍💥          |
| ----------------------------------------------------------- | --------------------------------------- |
| ✅ se puede probar directamente                             | 🧪 Necesito un componente para probar   |
| 🛠️ utiliza todo lo que no sean ciclos de vida e inyecciones | 🔄 utiliza ciclos de vida o inyecciones |

Descubrimos dos categorías de composables: **independientes** y **dependientes**. Los independientes se pueden probar de forma similar a las funciones normales. Los dependientes, están ligados al sistema de componentes y los hooks de ciclo de vida de Vue, requieren un enfoque más matizado.
