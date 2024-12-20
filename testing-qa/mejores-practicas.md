# 🧪 Mejores prácticas para nuestros testing en JS

## 🥇 La Regla de Oro

¡Haz que tus tests sean tus mejores aliados! 🫱🏾‍🫲🏽 A diferencia del código de producción, los tests no necesitan ser complejos ni llenos de abstracciones. Diseña tus pruebas de manera **simple y directa**, para que cualquiera pueda entender el propósito con solo mirarlas.

### 🔎 ¿Por qué es importante?

Cuando los tests son complicados, se vuelven una carga para el equipo. 🧠 Nuestros cerebros ya están ocupados con el código de producción, y añadir más comlejidad para ralentizar el trabajo o, peor aún, desmotivar a hacer testing.

### 💡 El truco está en la simplicidad

- Diseña tus tests como algo intuitivo y ligero, tan sencillo como editar un archivo HTML.
- Evita convertirlos en algo tan difícil como resolver una ecuación compleja.

### 🛠️ Cómo lograrlo:

1. Escoge herramientas y técnicas que sean fáciles de user y ofrezcan un buen retorno de inversión.
2. Prioriza lo esencial: prueba solo lo necesario.
3. no temas abandonar tests innecesarion si complican más de lo que ayudan.

Los tests deben sentirse como un asistente que te ayuda a avanzar, no como un obstáculo. Mantenlos ágiles, agradables, y sobre todo, útiles. ¡Testing ligero, equipo felíz! 🚀

## 🗒️ Nombra tus test con 3 partes clave

El nombre del test debe ser tan claro que cualquiera pueda entenderlo, desde un tester hasta tu _yo del futuro_ en un par de años. 🕰️ Para lograrlo, asegúrate de incluir estas tres partes:

### 1️⃣ ¿Qué estas testeando?

Identifica el componente, función o unidad que será evaluada. Ejemplo: `ProductsService.addNewProduct`.

### 2️⃣ ¿En qué escenario?

Describe las condiciones específicas bajo las cuales estás realizando la prueba. Ejemplo: **"Cuando no se pasa ningún precio al método"**.

### 3️⃣ ¿Qué esperas como resultado?

Indica el comportamiento esperado de la aplicación. Ejemplo: **"El producto no está aprobado"**.

### 🛠️ Así se ven en código:

```javascript
describe("Products Service", function () {
  describe("Add new product", function () {
    it("when no price is specified, then the product status is pending approval", () => {
      const newProduct = new ProductService().add(...);
      expect(newProduct.status).to.equal('pendigApproval');
    });
  });
});
```

### 🚨 ¿Por qué es importante?

Imagina que un despliegue falla y el único mensaje que ves es: _"Agregar producto falló"_. 😢 Eso no te dice nada útil. Sin embargo, un nombre bien estructurado como el del ejemplo anterior te permite identificar rápidamente qué salió mal y por qué.

### ✨ Beneficios de hacerlo bien:

- Claridad instantánea para todos los involucrados.
- Diagnóstico más rápido de problemas.
- Tests que realmente comunican lo necesario.

Dale a tus tests nombres que hablen por sí mismos. ¡La simplicidad es poder! 💪🏽

## 📚 Estrucura tus tests con el patrón AAA(Ajustar, Actuar y Afirmar)

Para mantener tus tests claros y fáciles de entender, utiliza el patrón AAA (Arrange, Act & Assert). Este enfoque divide cada prueba en tres secciones bien definidas:

### 1️⃣ Ajustar

Configura el entorno necesario para ejecutar el test. Esto incluye instanciar objetos, configurar mocks o stubs y preparar datos relevantes.

### 2️⃣ Actuar

Ejecuta la acción o método que quieres probar. Esta sección suele ser breve, generalmente una sola línea de código.

### 3️⃣ Afirmar

Verifica que los resultados coincidan con lo que esperas. Aquí validas que el comportamiento de tu código sea correcto, también con una o pocas líneas.

### 💡 Ejemplo claro y limpio

```javascript
describe("Customer classifier", () => {
  test("When customer spent more than $500, should be classified as premium", () => {
    // Ajustar
    const customerToClassify = { spent: 505, joined: new Date(), id: 1 };
    const DBStub = sinon
      .stub(dataAccess, "getCustomer")
      .reply({ id: 1, classification: "regular" });

    // Actuar
    const receivedClassification =
      customerClassifier.classifyCustomer(customerToClassify);

    // Afirmar
    expect(receivedClassification).toMatch("premium");
  });
});
```

### 🚨 ¿Qué pasa si no sigues este patrón?

Sin una estructura clara, tus tests pueden volverse confusos y difíciles de mantener. Imagina un bloque de código donde todo está mezclado: entender qué hace o dónde está el error será un desafío.

### 👎 Ejemplo desordenado

```javascript
test("Should be classified as premium", () => {
  const customerToClassify = { spent: 505, joined: new Date(), id: 1 };
  const DBStub = sinon
    .stub(dataAccess, "getCustomer")
    .reply({ id: 1, classification: "regular" });
  const receivedClassification =
    customerClassifier.classifyCustomer(customerToClassify);
  expect(receivedClassification).toMatch("premium");
});
```

### ✨ Beneficios del patrón AAA

- Claridad inmediata para cualquier lector, incluso si no está familiarizado con el código.
- Separación de responsabilidades: cada sección tiene un propósito claro.
- Fácil mantenimiento y depuración de tests.

Diseña tus pruebas para que sean tan simples como leer un buen libro. ¡Tu _futuro yo_ te lo agradecerá! 📖😊

## 📘 Escribe pruebas claras en el lenguaje del producto: ¡usa BDD!

Para facilitar la comprensión de tus pruebas, adopta un enfoque declarativo basado en **BDD (Behavior-Driven Development)**. Este estilo te permite expresar las expectativas del test de manera humana y legible, haciendo que cualquiera pueda entender el propósito del código al instante.

### 🔑 Por qué usar BDD en tus tests:

- 📖 Claridad: Evita la lógica compleja en los tests, lo que reduce el esfuerzo mental necesario para entenderlos.
- 🧰 Compatibilidad: Herramientas como Chai y Jest ofrecen métodos como expect o should que facilitan la escritura de aserciones intuitivas.
- 🛠️ Extensibilidad: Si necesitas validaciones específicas, puedes extender Jest con custom matchers o crear un plugin para Chai.

### 💡 Cómo hacerlo correctamente:

```javascript
it("When asking for an admin, ensure only ordered admins in results", () => {
  // Presuponiendo que hemos agregado "admin1", "admin2" y "user1" como usuarios
  const allAdmins = getUsers({ adminOnly: true });

  // Validamos de manera declarativa que el resultado contenga solo los administradores en orden
  expect(allAdmins)
    .to.include.ordered.members(["admin1", "admin2"])
    .but.not.include.ordered.members(["user1"]);
});
```

### 🟢 ¿Qué ganamos?

- Legibilidad: A simple vista, entendemos qué hace el test sin necesidad de analizar código.
- Precisión: Las aserciones son claras y específicas, reduciendo errores de interpretación.

### 👎 Ejemplo Anti Patrón:

Este enfoque usa un estilo imperativo que mezcla lógica compleja, dificultando la comprensión del test:

```javascript
test("When asking for an admin, ensure only ordered admins in results", () => {
  const allAdmins = getUsers({ adminOnly: true });

  let admin1Found,
    admin2Found = false;

  allAdmins.forEach((aSingleUser) => {
    if (aSingleUser === "user1") {
      assert.notEqual(aSingleUser, "user1", "A user was found and not admin");
    }
    if (aSingleUser === "admin1") {
      admin1Found = true;
    }
    if (aSingleUser === "admin2") {
      admin2Found = true;
    }
  });

  if (!admin1Found || !admin2Found) {
    throw new Error("Not all admins were returned");
  }
});
```

### 🔴 Problemas con este enfoque:

- Complejidad: Combina validaciones con iteraciones y condiciones lógicas, dificultando el mantenimiento.
- Errores: La mezcla de responsabilidades puede provocar bugs y hacer que el test sea frágil ante cambios.

### ✨ Beneficios de usar BDD en tus tests:

- Tests que "hablan el idioma del producto".
- Mayor colaboración entre desarrolladores, testers y stakeholders.
- **¡Menos excusas para ignorar tests!** 🛑 En lugar de marcar tests molestos con `.skip()`, crearás pruebas más amigables para todos.

Adopta un enfoque declarativo y construye un código que no solo pase los tests, ¡sino que también pase la prueba del tiempo! ⏳ 😊

## 📘 Pruebas de Caja Negra: Enfócate en métodos públicos

Adopta el enfoque de testing de comportamiento (behavioral testing), donde verificas únicamente los métodos públicos de una clase o componente. Esto asegura que estás probando el resultado esperado sin depender de la implementación interna, reduciendo así el mantenimiento y evitando falsos positivos.

### 🔑 Por qué usar pruebas de caja negra:

- 💡 **Reducción de mantenimiento**: Los tests no se rompen ante refactors menores o cambios internos.
- 🎯 **Foco en lo importante**: Se prueban los resultados esperados y no detalles de implementación.
- 🚦 **Menos falsos positivos**: Solo fallan si la funcionalidad pública realmente no cumple con el comportamiento esperado.

### 💡 Cómo hacerlo correctamente:

```javascript
class ProductService {
  getPrice(productId) {
    const desiredProduct = DB.getProduct(productId);
    const finalPrice = desiredProduct.price * 1.2; // Calcula el IVA internamente
    return finalPrice;
  }
}

it("Should return the final price including VAT for a given product", () => {
  // Configuración simulada de datos
  const mockDB = {
    getProduct: (id) => ({ id, price: 100 }), // Retorna un producto ficticio
  };
  const productService = new ProductService();
  sinon.stub(DB, "getProduct").callsFake(mockDB.getProduct);

  const finalPrice = productService.getPrice(1);

  expect(finalPrice).to.equal(120); // Valida el precio con IVA incluido
  DB.getProduct.restore();
});
```

### 🟢 ¿Qué ganamos?

- **Enfoque en la funcionalidad**: Validamos que el método público produce el resultado correcto.
- **Resiliencia ante cambios internos**: El test no se rompe si cambias cómo se calcula el IVA o el nombre del método interno.

### 👎 Ejemplo Anti Patrón:

El siguiente test prueba métodos internos que no son relevantes para la funcionalidad pública:

```javascript
class ProductService {
  calculateVATAdd(priceWithoutVAT) {
    return { finalPrice: priceWithoutVAT * 1.2 };
  }
  getPrice(productId) {
    const desiredProduct = DB.getProduct(productId);
    const finalPrice = this.calculateVATAdd(desiredProduct.price).finalPrice;
    return finalPrice;
  }
}

it("White-box test: When the internal method gets 0 VAT, it returns 0 response", () => {
  expect(new ProductService().calculateVATAdd(0).finalPrice).to.equal(0);
});
```

### 🔴 Problemas con este enfoque:

- Fragilidad: Cambiar el nombre o formato del método calculateVATAdd rompe el test, incluso si el método público sigue funcionando correctamente.
- Tiempo desperdiciado: Probar métodos internos agrega esfuerzo de mantenimiento sin aportar valor real.

### ✨ Beneficios de probar solo métodos públicos:

1. Menos ruido: Los falsos positivos disminuyen drásticamente.
2. Mejor colaboración: Otros desarrolladores entienden mejor lo que realmente importa: el comportamiento público.
3. Flexibilidad: Los cambios internos no afectan los tests, permitiendo refactors sin miedo.

¡Concéntrate en probar el qué y no el cómo! 🛠️ Esto hará que tus pruebas sean más confiables, mantenibles y alineadas con las expectativas del producto. 😊

## 📘 Eligiendo los dobles de prueba correctamente: Prefiere stubs y spies sobre mocks

Los **dobles de prueba** (mocks, stubs, spies) son útiles, pero pueden aumentar el acoplamiento si no se eligen correctamente. Este enfoque ayuda a priorizar pruebas basadas en requisitos funcionales y reduce el mantenimiento de los tests.

### 🔑 Cuándo usar cada doble de prueba:

1.**Stub**: Simula una dependencia externa y devuelve valores predefinidos.

- 🛠️ Útil para: Simular respuestas de APIs externas o manejar errores.
- Ejemplo: Probar cómo la aplicación maneja un servicio de pagos caído.

2. **Spy**: Monitorea interacciones, como si un método fue llamado o cuántas veces.

- 🛠️ Útil para: Asegurar que una acción requerida (como enviar un email) ocurra.

3. **Mock**: Verifica estrictamente argumentos y comportamiento interno.

- ⚠️ Evítalo siempre que sea posible. Es propenso a romperse ante cambios menores.

### ❌ Ejemplo Anti Patrón:

Aquí el test está acoplado a detalles internos del sistema:

```javascript
it("Ensure DAL.deleteProduct is called with correct arguments", async () => {
  const dataAccessMock = sinon.mock(DAL);
  dataAccessMock
    .expects("deleteProduct")
    .once()
    .withArgs(DBConfig, theProductWeJustAdded, true, false); // Test interno
  new ProductService().deletePrice(theProductWeJustAdded);
  dataAccessMock.verify();
});
```

#### 🔴 Problemas:

- **Acoplamiento innecesario**: Test centrado en detalles internos como DAL.deleteProduct.
- **Mantenimiento pesado**: Cambios menores en argumentos internos rompen los tests.

### 👏 Ejemplo Correcto:

Este enfoque prueba el comportamiento funcional de la aplicación.

```javascript
it("When a valid product is deleted, ensure an email is sent", async () => {
  const spy = sinon.spy(Emailer.prototype, "sendEmail"); // Espía el método
  new ProductService().deletePrice(theProductWeJustAdded);

  // Verifica el comportamiento deseado
  expect(spy.calledOnce).to.be.true; // Confirma que se envió el email
  Emailer.prototype.sendEmail.restore(); // Limpia el espía
});
```

#### 🟢 Ventajas:

- **Enfoque en los requisitos**: Verifica que se envió un email, que probablemente sea un requisito funcional.
- **Menor acoplamiento**: No se centra en los detalles de implementación de `deletePrice`.

### 💡 Caso práctico con Stubs:

Simula el comportamiento de un servicio externo (API de pagos):

```javascript
it("Should return an error message when payment service is down", async () => {
  const paymentServiceStub = sinon.stub(
    PaymentService.prototype,
    "processPayment"
  );
  paymentServiceStub.rejects(new Error("Service Unavailable")); // Simula error de servicio

  const productService = new ProductService();
  const result = await productService.purchaseProduct("product123");

  expect(result).to.equal("Payment failed, please try again later.");
  paymentServiceStub.restore(); // Limpia el stub
});
```

#### 🟢 Ventajas:

- **Simulación de escenarios reales**: Verifica cómo responde la app ante fallos externos.
- **Alineado con requisitos funcionales**: Este caso probablemente esté en el documento de requisitos.

### Conclusión:

- Usa **spies** para verificar interacciones funcionales.
- Usa **stubs** para simular escenarios externos.
- Evita **mocks**, excepto en casos muy específicos, ya que tienden a acoplarse a los detalles internos.
- **Prueba comportamientos, no implementaciones internas**. Esto asegura tests más robustos y mantenibles. 😊

## No uses `foo`, usa datos realistas

### 🛠️ Hazlo Mejor:

Los bugs suelen surgir con entradas inesperadas o específicas. Si tus datos de prueba se parecen a los reales, aumentas las posibilidades de detectar errores temprano. Usa librerías como [Faker](https://fakerjs.dev/) para generar datos pseudo-reales, como nombres de productos, precios, correos electrónicos o cualquier otro dato relevante. Además, considera usar datos aleatorios o incluso importar datos reales de producción (si es seguro hacerlo).

### 🔑 Por qué es importante:

- 💡 **Detecta bugs pronto**: Entradas realistas pueden revelar caminos no explorados.
- 🚨 **Evita falsos positivos**: Datos como "foo" o "bar" no cubren casos extremos o reales.
- 🔄 **Explora todas las ramas**: Datos variados activan más validaciones y lógicas.

### ✏ Ejemplo Correcto:

```javascript
it("Better: When adding new valid product, get successful confirmation", async () => {
  const addProductResult = addProduct(
    faker.commerce.productName(), // Nombre realista
    faker.random.number() // Precio realista
  );
  expect(addProductResult).to.be.true;
});
```

🎉 Resultado: Los datos generados al azar pueden provocar que el código siga caminos inesperados, descubriendo bugs que no se habían previsto.

### ❌ Qué evitar:

No uses cadenas simples como "Foo" o "Bar" que nunca pondrán a prueba las validaciones reales.

### 👎 Ejemplo Anti Patrón:

```javascript
test("Wrong: When adding new product with valid properties, get successful confirmation", async () => {
  const addProductResult = addProduct("Foo", 5);
  expect(addProductResult).toBe(true); // Nunca prueba validaciones de nombres con espacios.
});
```

### ✨ Beneficios de usar datos realistas:

- 📊 **Cobertura completa**: Validas más casos con datos variados.
- 🔒 **Menos sorpresas en producción**: Las pruebas reflejan mejor las condiciones reales.
- 🛠️ **Descubrimiento temprano de bugs**: Puedes encontrar errores antes de que lleguen a usuarios finales.

¡Recuerda! Los datos de prueba deben parecerse al mundo real para maximizar la eficacia de tus tests. 🚀

## 📘 Tests Basados en Propiedades: Descubre más bugs con menos esfuerzos

A veces, cubrir todas las combinaciones posibles de datos de entrada en pruebas manuales es poco práctico. Los tests basados en propiedades pueden automatizar esto, enviando múltiples combinaciones automáticamente para detectar fallos inesperados. ¡Haz que tus tests trabajen más por ti! 🚀

### 🔑 ¿Por qué usar tests basados en propiedades?

- 💡 **Cobertura amplia**: Aumenta las probabilidades de encontrar bugs al probar miles de combinaciones automáticamente.
- 🕵️ **Descubrimiento de fallos inesperados**: Identifica rutas que no habías considerado durante el diseño de tus tests.
- ⏱️ **Eficiencia**: Escribe menos tests manuales y consigue resultados más completos.

### 💡 Cómo hacerlo correctamente

Utiliza librerías como [fast-check](https://fast-check.dev/) para generar entradas aleatorias válidas que prueben muchas combinaciones.

```javascript
import fc from "fast-check";

describe("Product service", () => {
  describe("Adding new", () => {
    // Este test ejecutará automáticamente 100 combinaciones de entradas válidas.
    it("Add new product with random yet valid properties, always successful", () =>
      fc.assert(
        fc.property(fc.integer(), fc.string(), (id, name) => {
          expect(addNewProduct(id, name).status).toEqual("approved");
        })
      ));
  });
});
```

#### 🟢 ¿Qué logramos con esto?

- **Validaciones robustas**: Probamos múltiples combinaciones con mínimos esfuerzos.
- **Prevención de fallos**: Descubrimos errores ocultos que no se detectan con datos predefinidos.
- **Automatización**: Librerías como fast-check hacen el trabajo pesado por ti.

### 👎 Ejemplo Anti Patrón: Elegir datos manualmente

```javascript
it("Add new product with a specific ID and name", () => {
  expect(addNewProduct(1, "iPhone").status).toEqual("approved");
});
```

#### 🔴 Problemas de este enfoque:

- **Cobertura limitada**: Solo probamos una combinación, dejando fuera otros escenarios potenciales.
- **Sesgo humano**: Las entradas elegidas manualmente suelen evitar casos que podrían romper el sistema.

### ✨ Beneficios de usar tests basados en propiedades:

- 🌍 **Cobertura completa**: Cubres escenarios que ni siquiera habías imaginado.
- 📉 **Menor esfuerzo**: Dejas que las herramientas automaticen las combinaciones.
- 🔬 **Mejor calidad del código**: Detectas errores antes de que lleguen a producción.

> [!TIP]
> Complementa esta técnica con datos realistas generados por librerías como Faker para escenarios aún más sólidos.

¡Automatiza, experimenta y encuentra bugs antes de que lo hagan tus usuarios! 🎉
