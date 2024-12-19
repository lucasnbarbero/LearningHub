# 10 reglas para nombrar las pruebas

El camino hacia una mejor prueba, comienza con algo simple: cómo nombrar las pruebas.

Buenos nombres para las pruebas:

- Hacer que el conjunto de pruebas sea más fácil de mantener.
- Guiar hacia la redacción de pruebas que se centren en el comportamiento del usuario.
- Mejorar la calidad y la legibilidad para el equipo.

## Regla 1: Utilizar siempre `should` + verbo

Cada nombre de prueba debe comenzar con `should` seguido de un verbo de acción

```typescript
// ❌ Bad
it("displays the error message", () => {});
it("modal visibility", () => {});
it("form validation working", () => {});

// ✅ Good
it("should display error message when validation fails", () => {});
it("should show modal when trigger button is clicked", () => {});
it("should validate form when user submits", () => {});
```

#### Patrón genérico: `should [verbo] [resultado esperado]`

## Regla 2: Incluir el evento desencadenante

Especificar qué causa el comportamiento que estamos probando:

```typescript
// ❌ Bad
it("should update counter", () => {});
it("should validate email", () => {});
it("should show dropdown", () => {});

// ✅ Good
it("should increment counter when plus button is clicked", () => {});
it("should show error when email format is invalid", () => {});
it("should open dropdown when toggle is clicked", () => {});
```

#### Patrón genérico: `should [verbo] [resultado esperado] when [evento disparador]`

## Regla 3: Agrupar pruebas relacionadas

Utilizar bloques de descripción para crear jerarquías de pruebas claras

```typescript
// ❌ Bad
describe("AuthForm", () => {
  it("should test empty state", () => {});
  it("should test invalid state", () => {});
  it("should test success state", () => {});
});

// ✅ Good
describe("AuthForm", () => {
  describe("when form is empty", () => {
    it("should disable submit button", () => {});
    it("should not show any validation errors", () => {});
  });

  describe("when submitting invalid data", () => {
    it("should show validation errors", () => {});
    it("should keep submit button disabled", () => {});
  });
});
```

#### Patrón genérico:

```typescript
describe("[componente/funcionalidad]", () => {
  describe("when [condición específica]", () => {
    it("should [comportamiento esperado]", () => {});
  });
});
```

## Regla 4: El nombre del estado cambia explícitamente

Describir claramente los estado antes y después en los nombres de las pruebas

```typescript
// ❌ Bad
it("should change status", () => {});
it("should update todo", () => {});
it("should modify permissions", () => {});

// ✅ Good
it("should change status from pending to approved", () => {});
it("should mark todo as completed when checkbox clicked", () => {});
it("should upgrade user from basic to premium", () => {});
```

#### Patrón genérico: `should change [atributo] from [estado inicial] to [estado final]`

## Regla 5: Describie claramente el comportamiento

Incluir estados de carga y resultados para operaciones asíncronas

```typescript
// ❌ Bad
it("should load data", () => {});
it("should handle API call", () => {});
it("should fetch user", () => {});

// ✅ Good
it("should show skeleton while loading data", () => {});
it("should display error message when API call fails", () => {});
it("should render profile after user data loads", () => {});
```

#### Patrón genérico: `should [verbo] [resultado esperado] [durante/despues] [operacion asincrona]`

## Regla 6: Nombrar específicamente los casos de error

Ser expícito sobre el tipo de error y sus causas

```typescript
// ❌ Bad
it("should show error", () => {});
it("should handle invalid input", () => {});
it("should validate form", () => {});

// ✅ Good
it('should show "Invalid Card" when card number is wrong', () => {});
it('should display "Required" when password is empty', () => {});
it("should show network error when API is unreachable", () => {});
```

#### Patrón genérico: `should show [mensaje de error] when [condicion]`

## Regla 7: Utilizar un lenguaje comercial, no términos técnicos

Escribir pruebas utilizando lenguaje del dominio en lugar de detalles de implementación.

```typescript
// ❌ Bad
it("should update state", () => {});
it("should dispatch action", () => {});
it("should modify DOM", () => {});

// ✅ Good
it("should save customer order", () => {});
it("should update cart total", () => {});
it("should mark order as delivered", () => {});
```

#### Patrón genérico: `should [accion] [entidad]`

## Regla 8: Incluir condiciones previas importantes

Especificar las condiciones que afectan el comportamiento que estamos probando.

```typescript
// ❌ Bad
it("should enable button", () => {});
it("should show message", () => {});
it("should apply discount", () => {});

// ✅ Good
it("should enable checkout when cart has items", () => {});
it("should show free shipping when total exceeds $100", () => {});
it("should apply discount when user is premium member", () => {});
```

#### Patrón genérico: `should [comportamiento esperado] when [prediccion]`

## Regla 9: Nombrar pruebas de la retroalimentación de la UI desde la perspectiva del usuario

Describir los cambios visuales tal como los percibirá el usuario.

```typescript
// ❌ Bad
it("should set error class", () => {});
it("should toggle visibility", () => {});
it("should update styles", () => {});

// ✅ Good
it("should highlight search box in red when empty", () => {});
it("should show green checkmark when password is strong", () => {});
it("should disable submit button while processing", () => {});
```

#### Patrón genérico: `should [cambio visual] when [accion del usuario]`

## Regla 10: Estructurar flujos de trabajo complejos paso a paso

Dividir los procesos complejos en pasos claros.

```typescript
// ❌ Bad
describe("Checkout", () => {
  it("should process checkout", () => {});
  it("should handle shipping", () => {});
  it("should complete order", () => {});
});

// ✅ Good
describe("Checkout Process", () => {
  it("should first validate items are in stock", () => {});
  it("should then collect shipping address", () => {});
  it("should finally process payment", () => {});

  describe("after successful payment", () => {
    it("should display order confirmation", () => {});
    it("should send confirmation email", () => {});
  });
});
```

#### Patrón genérico

```typescript
describe("[Complex Process]", () => {
  it("should first [initial step]", () => {});
  it("should then [next step]", () => {});
  it("should finally [final step]", () => {});

  describe("after [key milestone]", () => {
    it("should [follow-up action]", () => {});
  });
});
```

## Ejemplo completo

Aquí mostramos un ejemplo completo que muestra cómo combinar todas las reglas:

```typescript
// ❌ Bad
describe("ShoppingCart", () => {
  it("test adding item", () => {});
  it("check total", () => {});
  it("handle checkout", () => {});
});

// ✅ Good
describe("ShoppingCart", () => {
  describe("when adding items", () => {
    it("should add item to cart when add button is clicked", () => {});
    it("should update total price immediately", () => {});
    it("should show item count badge", () => {});
  });

  describe("when cart is empty", () => {
    it("should display empty cart message", () => {});
    it("should disable checkout button", () => {});
  });

  describe("during checkout process", () => {
    it("should validate stock before proceeding", () => {});
    it("should show loading indicator while processing payment", () => {});
    it("should display success message after completion", () => {});
  });
});
```

## Lista de verificación del nombre de la prueba n.°

Antes de confirmar su prueba, verifique que su nombre:

- Empieza con "debería"/`should`
- Utiliza un verbo de acción claro
- Especifica la condición de activación
- Utiliza lenguaje comercial
- Describe el comportamiento visible
- ¿Es lo suficientemente específico para la depuración?
- Agrupa lógicamente con pruebas relacionadas

## Conclución

La denominación inteligente de las pruebas es un elemento fundamental en el panorama más amplio de la redacción de mejores pruebas. Para mantener la coherencia en todo el equipo:

1. Documente sus convenciones de nomenclatura en detalle
2. Comparta estas pautas con todos los miembros del equipo.
3. Integre las pautas en su flujo de trabajo de desarrollo

Para equipos que utilizan herramientas de IA como GitHub Copilot:

- Incorpore estas pautas en la documentación de su proyecto
- Vincula el archivo Markdown que contiene estas reglas a Copilot
- Esta integración permite a Copilot sugerir nombres de pruebas alineados con sus convenciones

Para obtener más información sobre como vincular la documentación a Copilot, consulte: [Los experimentos de VSCode](https://visualstudiomagazine.com/Articles/2024/09/09/VS-Code-Experiments-Boost-AI-Copilot-Functionality.aspx).
