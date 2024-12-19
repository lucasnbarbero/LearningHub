# ¿Qué es la trazabilidad?

La trazabilidad de software se refiere a la capacidad de rastrear y seguir el desarrollo, los cambios y las relaciones entre los elementos del software a lo largo de su ciclo de vida.
Es una práctica esencial para comprender cómo los diferentes componentes de un sistema están interconectados, facilitando la identificación de dependencias y el impacto de los cambios en el software.

### Objetivos principales:

- **Coherencia**: Garantizar que los requisitos, el diseño, el código y las pruebas estén alineados.
- **Mantenimiento**: Facilitar la localización de errores y la evaluación de cambios en el sistema.
- **Cumplimiento**: Ayudar a cumplir con estándares y normativas del sector.

## Elementos clave de la trazabilidad

1. **Requisitos**

- Permite rastrear la evolución de los requisitos desde su definición hasta su implementación.
  - Ejemplo: Un requisito como "El sistema debe permitir iniciar sesión con autenticación de dos factores" puede ser rastreado desde la documentación inicial hasta las pruebas que lo validan.

2. **Diseño**

- Mapea los requisitos con los componentes del diseño que los implementan.

  - Ejemplo: El diseño de un formulario de inicio de sesión debe reflejar el requisito de autenticación segura.

3. **Código fuente**

- Identifica cómo el diseño se traduce en líneas de código específicas.

  - Ejemplo: Relacionar una clase o función en el código con el módulo que satisface un requisito.

4. **Pruebas**

- Vincula los requisitos y los casos de prueba para garantizar que el software se valide correctamente.
  - Ejemplo: Una prueba que valida el inicio de sesión seguro debe estar asociada al requisito correspondiente.

5. **Documentación**

- Asegura que la documentación técnica y de usuario esté actualizada y alineada con el sistema.
  - Ejemplo: La guía del usuario debe reflejar cómo activar la autenticación de dos factores si esto está implementado.

## Características clave de trazabilidad

1. **Registro confiable**: Proporciona un historial detallado de eventos y acciones ocurridas en el sistema, útil para auditorías y resolución de problemas.
2. **Optimización de tareas críticas**: Ayuda a gestionar tareas complejas como el despliegue, las actualizaciones y el mantenimiento continuo.
3. **Gestión del ciclo de vida completo**: Rastrear cada paso del desarrollo, desde la idea inicial hasta el despliegue, facilita una visión integral del proceso.
4. **Identificación y resolución de fallos**: Mejora la capacidad de identificar el origen de problemas, reduciendo tiempos de diagnóstico.
5. **Visibilidad y alineación**: Ofrece una visión completa de cómo los diferentes elementos del sistema trabajan juntos para cumplir los objetivos del proyecto.

## Tipos de trazabilidad

1. **Trazabilidad hacia adelante (Forward Traceability)**:
   Rastrea cómo un requisito inicial influye en las etapas posteriores, como el diseño, la implementación y las pruebas.
2. **Trazabilidad hacia atrás (Backward Traceability)**:
   Permite rastrear un elemento (por ejemplo, un caso de prueba o código fuente) hasta su requisito original.
3. **Trazabilidad bidireccional**:
   Combina ambas direcciones, proporcionando un seguimiento completo entre requisitos, diseño, código y pruebas.

## Beneficios de implementar trazabilidad

- **Reducción de riesgos**: Minimiza la posibilidad de implementar requisitos no necesarios o no alineados con los objetivos del negocio.
- **Cumplimiento normativo**: Facilita la demostración de conformidad con estándares como ISO 9001, CMMI, o regulaciones específicas del sector.
- **Colaboración mejorada**: Mejora la comunicación entre equipos de desarrollo, QA y gestión de proyectos.

## Ejemplo práctico de trazabilidad

Supongamos que estás desarrollando una nueva funcionalidad para un sistema de gestión de usuarios. El flujo de trazabilidad podría verse así:

1. **Requisito**: "Permitir que los administradores bloqueen cuentas inactivas después de 90 días."
2. **Diseño**: El equipo decide agregar un estado "inactivo" a la tabla de usuarios en la base de datos.
3. **Código**: Se implementa una función en el backend para verificar y cambiar el estado de las cuentas.
4. **Pruebas**: Se crean casos de prueba unitarios y de integración para validar la funcionalidad.
5. **Documentación**: Se actualizan las guías del usuario y la documentación técnica.

## Herramientas para implementar trazabilidad de software

1. **Gestión de Requisitos**: Estas herramientas ayudan a documentar y rastrear los requisitos del sistema desde su concepción hasta su implementación:
   - Jira
     - Permite rastrear requisitos como historias de usuario, vincularlos a tareas de desarrollo y pruebas.
     - Integra flujos de trabajo personalizados que facilitan la trazabilidad entre etapas.
2. **Gestión de Pruebas**: Estas herramientas aseguran que los casos de prueba estén vinculados con los requisitos que validan:
   - **TestRail**:
     - Ofrece una visión clara de los casos de prueba y sus resultados, vinculados directamente a los requisitos.
     - Permite generar reportes de cobertura de pruebas.
   - **Zephyr (para Jira)**:
     - Se integra perfectamente con Jira para conectar pruebas y requisitos en un solo flujo.
   - **Xray (para Jira)**:
     - Una potente extensión que soporta trazabilidad bidireccional entre pruebas y requisitos.
3. **Control de Versiones**: Vincular cambios en el código con historias de usuario y requisitos es fundamental para la trazabilidad:
   - Git/GitHub/GitLab
     - Los mensajes de commit pueden incluir identificadores de requisitos (por ejemplo, claves de Jira).
     - Las "Pull Requests" permiten detallar cómo se implementa un requisito específico.
   - Azure DevOps:
     - Permite rastrear desde requisitos hasta implementaciones en código y pruebas automatizadas.
4. **Gestión Integral del Ciclo de Vida del Software (ALM)**: Herramientas que gestionan todas las fases del ciclo de vida, desde la planificación hasta el despliegue:
   - Azure DevOps:
     - Proporciona trazabilidad desde la planificación de requisitos, la implementación, las pruebas y los despliegues.
     - Soporta trazabilidad bidireccional y reportes automáticos.
   - Atlassian Suite (Jira, Confluence, Bitbucket):
     - Una solución completa que vincula tareas, código, pruebas y documentación en un solo ecosistema.
   - Polarion ALM:
     - Herramienta potente para trazabilidad en sectores regulados.
5. **Documentación y Colaboración**: La trazabilidad también implica que los equipos compartan información de manera clara:
   - Confluence:
     - Permite documentar requisitos, diseño y decisiones técnicas, vinculándolos a tareas en Jira.
   - Notion:
     - Ofrece una solución flexible para rastrear requisitos y decisiones técnicas en proyectos pequeños o medianos.
   - GitBook:
     - Ideal para crear documentación pública o interna que detalle cómo se implementan y prueban los requisitos.

## Automatización de la trazabilidad

Para proyectos más avanzados, se pueden utilizar herramientas de automatización que garantizan un seguimiento consistente:

1. **Plantillas de Commit**:
   - Configurar un estándar para mensajes de commit que incluya el ID del requisito, ejemplo:

```bash
feat(auth): Add 2FA login validation [JIRA-123]
```

2. **Integración Continua (CI)**:
   - Jenkins, GitHub Actions o Azure Pipelines: Configura pipelines que rastreen la ejecución de pruebas asociadas a requisitos específicos.

## Beneficios adicionales de las herramientas

- **Visibilidad**: Tableros y reportes que muestran la relación entre requisitos, tareas y pruebas.
- **Auditorías más sencillas**: Cumplimiento con normativas específicas gracias a la trazabilidad clara y bien documentada.
- **Ahorro de tiempo**: Reduce el esfuerzo manual al rastrear dependencias y relaciones entre elementos.
