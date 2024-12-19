# ¿Cómo estructurar proyectos de Vue?

Esta sección cubre varias estructuras de proyectos de Vue para proyectos de distintos tamaños:

- Estructura plana para pequeños proyectos
- Diseño atómico para aplicaciones escalables
- Enfoque modular para proyectos más grandes
- Diseño segmentado de características para aplicaciones complejas
- Microfrontends para soluciones de nivel empresarial

## Introducción

Al iniciar un proyecto, una de las decisiones más importantes es cómo estructurarlo. Una buena estructura mejora la escalabilidad, la capacidad de mantenimiento y la colaboración dentro del equipo.

## 1. Estructura plana: para pequeños proyectos

Estructura de carpetas simple y plana podría ser la mejor opción para simplificar las cosas y evitar complejidad innecesaria.

```
/src
|-- /components
|   |-- BaseButton.vue
|   |-- BaseCard.vue
|   |-- PokemonList.vue
|   |-- PokemonCard.vue
|-- /composables
|   |-- usePokemon.js
|-- /utils
|   |-- validators.js
|-- /layout
|   |-- DefaultLayout.vue
|   |-- AdminLayout.vue
|-- /plugins
|   |-- translate.js
|-- /views
|   |-- Home.vue
|   |-- PokemonDetail.vue
|-- /router
|   |-- index.js
|-- /store
|   |-- index.js
|-- /assets
|   |-- /images
|   |-- /styles
|-- /tests
|   |-- ...
|-- App.vue
|-- main.js
```

### Pros y contras

| ✅ Ventajas                                          | ❌ Desventajas                                       |
| ---------------------------------------------------- | ---------------------------------------------------- |
| Fácil de mantener                                    | No escalable                                         |
| Configuración mínima                                 | Se vuelve desordenado a medida que el proyecto crece |
| Ideal para equipos pequeños o desarrollos personales | No es clara la separación de responsabilidades       |

## 2. Diseño atómico: organización escalable de componentes

Para aplicaciones Vue más grandes, la metodología de **diseño atómico** puede resultar ventajosa. Este enfoque organiza los componentes en una jerarquía desde el más simple al más complejo.

### La jerarquía atómica

- **Átomos**: Elementos básicos como botones e iconos.
- **Moléculas**: Grupos de átomos que forman componentes simples (por ejemplo, barras de búsqueda).
- **Organismos**: Componentes complejos formados por moléculas y átomos (por ejemplo, barras de navegación).
- **Plantillas**: Diseños de página que estructuran organismos sin contenido real.
- **Páginas**: Plantillas llenas de contenido real para formar páginas reales.

Este método garantiza la escalabilidad y la capacidad de mantenimiento, facilitando una transición fluida entre componentes simples y complejos.

```
/src
|-- /components
|   |-- /atoms
|   |   |-- AtomButton.vue
|   |   |-- AtomIcon.vue
|   |-- /molecules
|   |   |-- MoleculeSearchInput.vue
|   |   |-- MoleculePokemonThumbnail.vue
|   |-- /organisms
|   |   |-- OrganismPokemonCard.vue
|   |   |-- OrganismHeader.vue
|   |-- /templates
|   |   |-- TemplatePokemonList.vue
|   |   |-- TemplatePokemonDetail.vue
|-- /pages
|   |-- PageHome.vue
|   |-- PagePokemonDetail.vue
|-- /composables
|   |-- usePokemon.js
|-- /utils
|   |-- validators.js
|-- /layout
|   |-- LayoutDefault.vue
|   |-- LayoutAdmin.vue
|-- /plugins
|   |-- translate.js
|-- /router
|   |-- index.js
|-- /store
|   |-- index.js
|-- /assets
|   |-- /images
|   |-- /styles
|-- /tests
|   |-- ...
|-- App.vue
|-- main.js
```

### Pros y contras

| ✅ Ventajas                          | ❌ Contras                                          |
| ------------------------------------ | --------------------------------------------------- |
| Altamente escalable                  | Puede introducir sobrecarga en la gestión de capas. |
| Jerarquía de componentes organizada  | Complejidad inicial en la configuración             |
| Componentes reutilizables            | Podría ser excesivo para proyectos más pequeños.    |
| Mejora la colaboración entre equipos | ---                                                 |

## 3. Enfoque modular: organización basada en características

A medida que su proyecto se expande, considere una **arquitectura monolítica modular** . Esta estructura encapsula cada característica o dominio, lo que mejora la capacidad de mantenimiento y prepara el terreno para una posible evolución hacia los microservicios.

```
/src
|-- /core
|   |-- /components
|   |   |-- BaseButton.vue
|   |   |-- BaseIcon.vue
|   |-- /models
|   |-- /store
|   |-- /services
|   |-- /views
|   |   |-- DefaultLayout.vue
|   |   |-- AdminLayout.vue
|   |-- /utils
|   |   |-- validators.js
|-- /modules
|   |-- /pokemon
|   |   |-- /components
|   |   |   |-- PokemonThumbnail.vue
|   |   |   |-- PokemonCard.vue
|   |   |   |-- PokemonListTemplate.vue
|   |   |   |-- PokemonDetailTemplate.vue
|   |   |-- /models
|   |   |-- /store
|   |   |   |-- pokemonStore.js
|   |   |-- /services
|   |   |-- /views
|   |   |   |-- PokemonDetailPage.vue
|   |   |-- /tests
|   |   |   |-- pokemonTests.spec.js
|   |-- /search
|   |   |-- /components
|   |   |   |-- SearchInput.vue
|   |   |-- /models
|   |   |-- /store
|   |   |   |-- searchStore.js
|   |   |-- /services
|   |   |-- /views
|   |   |-- /tests
|   |   |   |-- searchTests.spec.js
|-- /assets
|   |-- /images
|   |-- /styles
|-- /scss
|-- App.vue
|-- main.ts
|-- router.ts
|-- store.ts
|-- /tests
|   |-- ...
|-- /plugins
|   |-- translate.js
```

### Pros y contras

| ✅ Ventajas                                           | ❌ Contras                                              |
| ----------------------------------------------------- | ------------------------------------------------------- |
| Mejora la escalabilidad                               | Posible duplicación de código                           |
| Características encapsuladas                          | Puede volverse complejo si no se gestiona adecuadamente |
| Colaboración en equipo más sencilla                   | Requiere disciplina en los límites del módulo.          |
| Se prepara para la futura transición a microservicios | ---                                                     |

## 4. Diseño por características: para aplicaciones complejas

El **diseño segmentado por funciones** es ideal para proyectos grandes y de largo plazo. Este enfoque divide la aplicación en diferentes capas, cada una con una función específica.

### Capas de diseño segmentadas por características

- **Aplicación**: configuraciones globales, estilos y proveedores.
- **Procesos**: Procesos comerciales globales, como flujos de autenticación de usuarios.
- **Páginas**: Páginas completas creadas utilizando entidades, funciones y widgets.
- **Widgets**: combina entidades y funciones en bloques de interfaz de usuario cohesivos.
- **Características**: Maneja interacciones de usuario que agregan valor.
- **Entidades**: Representa los principales modelos de negocio.
- **Compartido**: Utilidades y componentes reutilizables no relacionados con la lógica empresarial específica.

```
/src
|-- /app
|   |-- App.vue
|   |-- main.js
|   |-- app.scss
|-- /processes
|-- /pages
|   |-- Home.vue
|   |-- PokemonDetailPage.vue
|-- /widgets
|   |-- UserProfile.vue
|   |-- PokemonStatsWidget.vue
|-- /features
|   |-- pokemon
|   |   |-- CatchPokemon.vue
|   |   |-- PokemonList.vue
|   |-- user
|   |   |-- Login.vue
|   |   |-- Register.vue
|-- /entities
|   |-- user
|   |   |-- userService.js
|   |   |-- userModel.js
|   |-- pokemon
|   |   |-- pokemonService.js
|   |   |-- pokemonModel.js
|-- /shared
|   |-- ui
|   |   |-- BaseButton.vue
|   |   |-- BaseInput.vue
|   |   |-- Loader.vue
|   |-- lib
|   |   |-- api.js
|   |   |-- helpers.js
|-- /assets
|   |-- /images
|   |-- /styles
|-- /router
|   |-- index.js
|-- /store
|   |-- index.js
|-- /tests
|   |-- featureTests.spec.js
```

### Pros y contras

| ✅ Ventajas                         | ❌ Contras                                                |
| ----------------------------------- | --------------------------------------------------------- |
| Alta cohesión y clara separación    | Complejidad inicial en la comprensión de las capas        |
| Escalable y mantenible              | Requiere una planificación exhaustiva                     |
| Facilita la colaboración en equipo. | Es necesaria una aplicación coherente de las convenciones |

> Para más información, leer la [documentación oficial](https://feature-sliced.design/) de Features-Sliced Design.

## 5. Microfrontends: solución de nivel empresarial

Los **microfrontends** trasladan el concepto de microservicios al frontend. Diferentes equipos pueden trabajar en distintas secciones de una aplicación web de forma independiente, lo que permite un desarrollo y una implementación flexibles.

### Componentes clave

- **Shell de aplicación**: el controlador principal que maneja el diseño básico y el enrutamiento y conecta todos los microfrontends.
- **Interfaces de usuario descompuestas**: cada microfrontend se centra en una parte específica de la aplicación y puede desarrollarse con diferentes tecnologías.

### Pros y contras

| ✅ Ventajas                              | ❌ Contras                                             |
| ---------------------------------------- | ------------------------------------------------------ |
| Despliegues independientes               | Alta complejidad en la orquestación                    |
| Escalabilidad en equipos grandes         | Requiere una infraestructura robusta                   |
| Enfoque agnóstico en cuanto a tecnología | Posibles inconsistencias en la experiencia del usuario |

> [!WARNING] ¡Cuidado!
> Los microfrontends son más adecuados para proyectos grandes y complejos con varios equipos de desarrollo. Este enfoque puede generar una complejidad significativa y, por lo general, no es necesario para aplicaciones de tamaño pequeño a mediano.

## Conclusión

La selección de la estructura de proyecto adecuada depende del tamaño, la complejidad y la organización del equipo del proyecto. Cuanto más complejo sea el equipo o el proyecto, más se debe procurar una estructura que facilite la escalabilidad y la capacidad de mantenimiento.

### Cuadro comparativo

| Estructura                                 | Descripción                                                | ✅ Ventajas                                         | ❌ Contras                                                 |
| ------------------------------------------ | ---------------------------------------------------------- | --------------------------------------------------- | ---------------------------------------------------------- |
| **Estructura plana**                       | Estructura sencilla para proyectos pequeños                | Fácil de implementar                                | No es escalable y puede volverse desordenado.              |
| **Diseño atómico**                         | Estructura jerárquica basada en componentes                | Componentes escalables, organizados y reutilizables | Sobrecarga en la gestión de capas, complejidad inicial     |
| **Enfoque modular**                        | Estructura modular basada en características               | Funciones escalables y encapsuladas                 | Posible duplicación, requiere disciplina                   |
| **Diseño con características segmentadas** | Capas y sectores funcionales para proyectos de gran tamaño | Alta cohesión, clara separación                     | Complejidad inicial, requiere una planificación exhaustiva |
| **Microfrontends**                         | Despliegues independientes de componentes frontend         | Despliegues independientes, escalables              | Alta complejidad, requiere coordinación entre equipos      |

## Reglas generales y mejores prácticas

### Nombres de componentes base

Utilizar un prefijo para los componentes UI reutilizables

```
// ❌ Bad
components/
|-- MyButton.vue
|-- VueTable.vue
|-- Icon.vue

// ✅ Good
components/
|-- BaseButton.vue
|-- BaseTable.vue
|-- BaseIcon.vue
```

### Nombres de componentes acoplados

Agrupar los componentes relacionados

```
// ❌ Bad
components/
|-- TodoList.vue
|-- TodoItem.vue
|-- TodoButton.vue

// ✅ Good
components/
|-- TodoList.vue
|-- TodoListItem.vue
|-- TodoListItemButton.vue
```

### Orden de palabras en los nombres

Los nombres de los componentes deben comenzar con las palabras de nivel más alto y terminar con modificadores descriptivos:

```
// ❌ Bad
components/
|-- ClearSearchButton.vue
|-- ExcludeFromSearchInput.vue
|-- LaunchOnStartupCheckbox.vue

// ✅ Good
components/
|-- SearchButtonClear.vue
|-- SearchInputExclude.vue
|-- SettingsCheckboxLaunchOnStartup.vue
```

### Organizando pruebas

Decidir si queremos mantener las pruebas en una carpeta dedicada o junto con los componentes. Ambos enfoques son válidos, pero la coherencia es clave

#### Método 1: Carpeta separada

```
/vue-project
|-- src
|   |-- components
|   |   |-- MyComponent.vue
|   |-- views
|   |   |-- HomeView.vue
|-- tests
|   |-- components
|   |   |-- MyComponent.spec.js
|   |-- views
|   |   |-- HomeView.spec.js
```

#### Método 2: Archivos de prueba en línea

```
/vue-project
|-- src
|   |-- components
|   |   |-- MyComponent.vue
|   |   |-- MyComponent.spec.js
|   |-- views
|   |   |-- HomeView.vue
|   |   |-- HomeView.spec.js
```

## Recursos adicionales

- [Guía de estilo oficial de Vue.js](https://vuejs.org/style-guide/)
- [Micro Frontends: extensión de las ideas de microservicios al desarrollo de frontends](https://micro-frontends.org/)
- [Martin Fowler sobre los microfrontends](https://martinfowler.com/articles/micro-frontends.html)
- [Documentación oficial de diseño segmentado en funciones](https://feature-sliced.design/)
