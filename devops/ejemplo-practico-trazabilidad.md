# Flujo de Trabajo Automatizado para la Trazabilidad

En este artículo, aprenderás cómo configurar un flujo de trabajo automatizado utilizando **Azure DevOps** para un proyecto que incluye:

1. Un **frontend** desarrollado con **Vue.js**.
2. Un **backend** desarrollado con **Node.js** (usando Express).

A través de este flujo, implementaremos trazabilidad completa desde los requisitos hasta el despliegue, asegurando que cada cambio pueda rastrearse de manera eficiente.

## 1. Configuración del Proyecto en Azure DevOps

### Crear un espacio de trabajo

1. Ve a tu cuenta de Azure DevOps y crea un nuevo proyecto:

   - Nombre del proyecto: `FullStackExample`.
   - Configura el repositorio como **Git** y selecciona visibilidad privada o pública, según tu preferencia.

2. Activa los siguientes servicios:
   - **Boards**: Para gestionar tareas, requisitos y sprints.
   - **Repos**: Para el control de versiones de código.
   - **Pipelines**: Para CI/CD.
   - **Test Plans**: Para pruebas automatizadas.

## 2. Configuración del Repositorio

Crea dos repositorios dentro del proyecto:

- **FrontendRepo**: Para el código del frontend (Vue.js).
- **BackendRepo**: Para el código del backend (Node.js con Express).

### Convención de ramas

Adopta una estrategia de ramas como:

- `main`: Rama principal para producción.
- `develop`: Rama para desarrollo.
- `feature/*`: Ramas para nuevas funcionalidades.

## 3. Configuración del Frontend (Vue.js)

### Estructura del Proyecto

1. Crea un nuevo proyecto de Vue.js con **Vite**:

```bash
npm create vite@latest frontend --template vue-ts
cd frontend
npm install
```

2. Sube el código inicial a **FrontendRepo**:

```base
git init
git remote add origin <url-del-repo>
git add .
git commit -m "Initial commit"
git push -u origin main
```

---

### Pipeline de CI/CD para el Frontend

1. Crea un archivo llamado `.azure-pipelines.yml` en el repositorio **FrontendRepo** con este contenido:

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: "ubuntu-latest"

steps:
  - task: UseNode@2
    inputs:
      version: "16.x"

  - script: npm install
    displayName: "Install Dependencies"

  - script: npm run build
    displayName: "Build Vue App"

  - task: PublishBuildArtifacts@1
    inputs:
      PathtoPublish: "dist"
      ArtifactName: "frontend-dist"
```

2. Configura el pipeline en Azure Pipelines:

   - Ve a **Pipelines > New Pipeline**.
   - Selecciona el repositorio **FrontendRepo** y conecta el archivo **YAML**.

3. Agrega pasos de despliegue:

   - Usa **Azure Static Web Apps** o cualquier otro servicio de hosting para desplegar el contenido de la carpeta `dist`.

## 4. Configuración del Backend (Node.js con Express)

### Estructura del Proyecto

1. Crea un nuevo proyecto para el backend:

```bash
mkdir backend
cd backend
npm init -y
npm install express
```

2. Crea un archivo `index.js` básico:

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello, World!");
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

3. Sube el código inicial a **BackendRepo**:

```bash
git init
git remote add origin <url-del-repo>
git add .
git commit -m "Initial commit"
git push -u origin main
```

---

### Pipeline de CI/CD para el Backend

1. Crea un archivo llamado `.azure-pipelines.yml` en el repositorio **BackendRepo**:

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: "ubuntu-latest"

steps:
  - task: UseNode@2
    inputs:
      version: "16.x"

  - script: npm install
    displayName: "Install Dependencies"

  - script: npm test
    displayName: "Run Tests"

  - script: |
      echo "Deploying Backend"
      # Agrega comandos específicos para tu servidor o servicio de despliegue.
    displayName: "Deploy Backend"
```

2. Configura el pipeline en Azure Pipelines:

   - Ve a **Pipelines > New Pipeline**.
   - Selecciona el repositorio **BackendRepo** y conecta el archivo **YAML**.

3. Agrega pasos para despliegue en tu servidor o servicio cloud, como **Azure App Service** o **Docker Containers**.

## 5. Implementar Trazabilidad

### Conectar Work Items a Commits

1. Configura Azure Boards:
   - Crea un Epic: "Desarrollo FullStack".
   - Divide en Features: "Desarrollo Frontend" y "Desarrollo Backend".
   - Crea User Stories y tareas relacionadas.
2. Relación entre commits y work items: Usa el identificador de work item en los mensajes de commit:

```bash
git commit -m "feat: Add login endpoint [AB#456]"
```

3. Visualiza trazabilidad:
   - Desde cualquier Work Item, verás los commits, builds y despliegues relacionados.

## 6. Pruebas Automatizadas

- **Frontend**: Integra herramientas como Vitest o Cypress para pruebas de unidad y E2E.
- **Backend**: Usa Jest o Mocha para pruebas unitarias.

Agrega pruebas al pipeline y publica resultados con:

```yaml
- task: PublishTestResults@2
  inputs:
  testResultsFiles: '\*\*/test-results.xml'
  testRunTitle: "Test Results"
```

## 7. Dashboards y Reportes

1. Configura un Dashboard en **Azure DevOps** para rastrear:
   - Progreso de trabajo en **Boards**.
   - Estado de builds en **Pipelines**.
   - Cobertura de pruebas y errores.
2. Usa widgets como **Burn Down Chart** o **Pipeline Runs**.
