# Unidad: Configuración de CI/CD con GitHub Actions

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la configuración de un pipeline de CI/CD (Integración Continua y Despliegue Continuo) utilizando GitHub Actions para un sistema de carrito de compras desarrollado con Django. La implementación de CI/CD es crucial para asegurar que el código se integre y despliegue de manera automática y eficiente, minimizando errores humanos y acelerando el ciclo de desarrollo.

### Objetivos de aprendizaje

1. Comprender los conceptos fundamentales de CI/CD y su importancia en el desarrollo de software.
2. Configurar GitHub Actions para automatizar el proceso de integración y despliegue de una aplicación Django.
3. Crear y gestionar workflows en GitHub Actions.
4. Implementar pruebas automáticas y despliegues continuos.
5. Aplicar mejores prácticas en la configuración de CI/CD.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es configurar un pipeline de CI/CD utilizando GitHub Actions para el sistema de carrito de compras con Django. Este pipeline debe realizar las siguientes tareas:

1. **Integración Continua (CI)**:
   - Ejecutar pruebas unitarias y de integración.
   - Verificar la calidad del código mediante herramientas de análisis estático.
   - Construir la imagen Docker de la aplicación.

2. **Despliegue Continuo (CD)**:
   - Desplegar la aplicación en un entorno de staging para pruebas adicionales.
   - Desplegar la aplicación en un entorno de producción.

### Casos de uso

#### Caso de uso 1: Ejecución de pruebas automáticas

**Actor**: Desarrollador

**Descripción**: Cada vez que un desarrollador realiza un push a la rama principal del repositorio, GitHub Actions debe ejecutar automáticamente todas las pruebas unitarias y de integración para asegurar que los cambios no introduzcan errores.

**Flujo**:
1. El desarrollador realiza un push a la rama principal.
2. GitHub Actions ejecuta el workflow de CI.
3. Se ejecutan las pruebas unitarias y de integración.
4. Se genera un reporte de resultados.

#### Caso de uso 2: Despliegue en entorno de staging

**Actor**: Desarrollador

**Descripción**: Después de que las pruebas automáticas se ejecutan con éxito, la aplicación se despliega automáticamente en un entorno de staging para pruebas adicionales.

**Flujo**:
1. El workflow de CI se completa con éxito.
2. GitHub Actions ejecuta el workflow de CD para el entorno de staging.
3. La aplicación se despliega en el entorno de staging.

#### Caso de uso 3: Despliegue en entorno de producción

**Actor**: Administrador de sistemas

**Descripción**: Después de que las pruebas en el entorno de staging se completan con éxito, la aplicación se despliega automáticamente en el entorno de producción.

**Flujo**:
1. El administrador de sistemas aprueba el despliegue en producción.
2. GitHub Actions ejecuta el workflow de CD para el entorno de producción.
3. La aplicación se despliega en el entorno de producción.

### Diagramas de flujo

```mermaid
graph TD
    A[Push a la rama principal] --> B[Ejecutar pruebas automáticas]
    B --> C{Pruebas exitosas?}
    C -- Sí --> D[Desplegar en staging]
    D --> E[Pruebas en staging]
    E --> F{Pruebas exitosas?}
    F -- Sí --> G[Desplegar en producción]
    C -- No --> H[Notificar error]
    F -- No --> H
```

### Requisitos no funcionales

1. **Rendimiento**: El pipeline de CI/CD debe ejecutarse en un tiempo razonable para no afectar la productividad del equipo de desarrollo.
2. **Escalabilidad**: La configuración debe ser capaz de manejar múltiples desarrolladores trabajando en paralelo.
3. **Seguridad**: Asegurar que las credenciales y secretos utilizados en el pipeline no se expongan.
4. **Mantenibilidad**: El pipeline debe ser fácil de mantener y actualizar conforme cambian los requisitos del proyecto.

## Implementación en Python

### Explicación paso a paso del código

Para configurar GitHub Actions, necesitamos crear archivos de workflow en el directorio `.github/workflows` de nuestro repositorio. A continuación, explicaremos cada paso necesario para configurar CI/CD para nuestra aplicación Django.

#### Paso 1: Crear el archivo de workflow para CI

Crearemos un archivo llamado `ci.yml` en el directorio `.github/workflows` con el siguiente contenido:

```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:12
        env:
          POSTGRES_DB: test_db
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.8'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        env:
          DATABASE_URL: postgres://user:password@localhost:5432/test_db
        run: |
          python manage.py migrate
          python manage.py test
```

#### Paso 2: Crear el archivo de workflow para CD en staging

Crearemos un archivo llamado `cd-staging.yml` en el directorio `.github/workflows` con el siguiente contenido:

```yaml
name: CD to Staging

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      - name: Log in to Docker Hub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        run: |
          docker buildx build --platform linux/amd64 --push -t ${{ secrets.DOCKER_USERNAME }}/carrito-compras:staging .

      - name: Deploy to Staging
        run: |
          ssh -o StrictHostKeyChecking=no ${{ secrets.STAGING_SSH_USER }}@${{ secrets.STAGING_HOST }} << 'EOF'
          docker pull ${{ secrets.DOCKER_USERNAME }}/carrito-compras:staging
          docker-compose -f docker-compose.staging.yml up -d
          EOF
```

#### Paso 3: Crear el archivo de workflow para CD en producción

Crearemos un archivo llamado `cd-production.yml` en el directorio `.github/workflows` con el siguiente contenido:

```yaml
name: CD to Production

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      - name: Log in to Docker Hub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        run: |
          docker buildx build --platform linux/amd64 --push -t ${{ secrets.DOCKER_USERNAME }}/carrito-compras:latest .

      - name: Deploy to Production
        run: |
          ssh -o StrictHostKeyChecking=no ${{ secrets.PRODUCTION_SSH_USER }}@${{ secrets.PRODUCTION_HOST }} << 'EOF'
          docker pull ${{ secrets.DOCKER_USERNAME }}/carrito-compras:latest
          docker-compose -f docker-compose.production.yml up -d
          EOF
```

### Código fuente completo y comentado

A continuación, presentamos el código fuente completo y comentado de los archivos de workflow.

#### Archivo `ci.yml`

```yaml
name: CI

# Disparadores del workflow: push a la rama principal y pull requests
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    # El job se ejecuta en un contenedor Ubuntu
    runs-on: ubuntu-latest

    services:
      # Configuración del servicio de base de datos PostgreSQL
      postgres:
        image: postgres:12
        env:
          POSTGRES_DB: test_db
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      # Paso para clonar el repositorio
      - name: Checkout code
        uses: actions/checkout@v2

      # Paso para configurar Python
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.8'

      # Paso para instalar dependencias
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      # Paso para ejecutar las pruebas
      - name: Run tests
        env:
          DATABASE_URL: postgres://user:password@localhost:5432/test_db
        run: |
          python manage.py migrate
          python manage.py test
```

#### Archivo `cd-staging.yml`

```yaml
name: CD to Staging

# Disparador del workflow: push a la rama principal
on:
  push:
    branches:
      - main

jobs:
  deploy:
    # El job se ejecuta en un contenedor Ubuntu
    runs-on: ubuntu-latest

    steps:
      # Paso para clonar el repositorio
      - name: Checkout code
        uses: actions/checkout@v2

      # Paso para configurar Docker Buildx
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      # Paso para iniciar sesión en Docker Hub
      - name: Log in to Docker Hub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Paso para construir y enviar la imagen Docker
      - name: Build and push Docker image
        run: |
          docker buildx build --platform linux/amd64 --push -t ${{ secrets.DOCKER_USERNAME }}/carrito-compras:staging .

      # Paso para desplegar en staging
      - name: Deploy to Staging
        run: |
          ssh -o StrictHostKeyChecking=no ${{ secrets.STAGING_SSH_USER }}@${{ secrets.STAGING_HOST }} << 'EOF'
          docker pull ${{ secrets.DOCKER_USERNAME }}/carrito-compras:staging
          docker-compose -f docker-compose.staging.yml up -d
          EOF
```

#### Archivo `cd-production.yml`

```yaml
name: CD to Production

# Disparador del workflow: manual mediante workflow_dispatch
on:
  workflow_dispatch:

jobs:
  deploy:
    # El job se ejecuta en un contenedor Ubuntu
    runs-on: ubuntu-latest

    steps:
      # Paso para clonar el repositorio
      - name: Checkout code
        uses: actions/checkout@v2

      # Paso para configurar Docker Buildx
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      # Paso para iniciar sesión en Docker Hub
      - name: Log in to Docker Hub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Paso para construir y enviar la imagen Docker
      - name: Build and push Docker image
        run: |
          docker buildx build --platform linux/amd64 --push -t ${{ secrets.DOCKER_USERNAME }}/carrito-compras:latest .

      # Paso para desplegar en producción
      - name: Deploy to Production
        run: |
          ssh -o StrictHostKeyChecking=no ${{ secrets.PRODUCTION_SSH_USER }}@${{ secrets.PRODUCTION_HOST }} << 'EOF'
          docker pull ${{ secrets.DOCKER_USERNAME }}/carrito-compras:latest
          docker-compose -f docker-compose.production.yml up -d
          EOF
```

### Ejemplos de uso y pruebas unitarias

Para probar la configuración de CI/CD, podemos realizar los siguientes pasos:

1. **Ejecutar pruebas automáticas**: Realizar un push a la rama principal y verificar que GitHub Actions ejecute las pruebas automáticamente.
2. **Despliegue en staging**: Verificar que después de un push a la rama principal, la aplicación se despliegue en el entorno de staging.
3. **Despliegue en producción**: Ejecutar manualmente el workflow de despliegue en producción desde la interfaz de GitHub Actions y verificar que la aplicación se despliegue correctamente.

### Mejores prácticas y consideraciones de diseño

1. **Seguridad**: Utilizar secretos de GitHub para almacenar credenciales y otros datos sensibles.
2. **Modularidad**: Mantener los workflows separados para CI y CD para facilitar su mantenimiento y actualización.
3. **Notificaciones**: Configurar notificaciones para alertar al equipo de desarrollo sobre el estado de los workflows.
4. **Versionado**: Utilizar etiquetas y versiones para las imágenes Docker para facilitar el seguimiento y la reversión de cambios.
5. **Pruebas exhaustivas**: Asegurarse de que todas las pruebas relevantes se ejecuten en el pipeline de CI para detectar errores lo antes posible.

Con esta configuración de CI/CD, el equipo de desarrollo puede asegurarse de que el sistema de carrito de compras con Django se integre y despliegue de manera automática y eficiente, mejorando la calidad del código y acelerando el ciclo de desarrollo.