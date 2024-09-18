# Unidad: Manejo seguro de secretos con django-environ

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en el manejo seguro de secretos utilizando la librería `django-environ` en el contexto de un sistema de carrito de compras desarrollado con Django. La seguridad de los secretos, como claves API, contraseñas de bases de datos y otros datos sensibles, es crucial para proteger nuestra aplicación contra accesos no autorizados y posibles vulnerabilidades. Al finalizar esta unidad, serás capaz de:

1. Comprender la importancia del manejo seguro de secretos en aplicaciones web.
2. Configurar y utilizar la librería `django-environ` para gestionar variables de entorno de manera segura.
3. Implementar buenas prácticas para el almacenamiento y acceso a secretos en tu aplicación Django.
4. Realizar pruebas unitarias para asegurar que los secretos se manejan correctamente.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es integrar `django-environ` en nuestro proyecto Django para gestionar secretos de manera segura. Esto incluye la configuración de variables de entorno para almacenar información sensible y la modificación de los archivos de configuración de Django para utilizar estas variables.

### b. Casos de uso

1. **Configuración inicial del proyecto**: Al iniciar un nuevo proyecto Django, el desarrollador debe configurar `django-environ` para manejar secretos de manera segura.
2. **Acceso a secretos en el código**: Durante el desarrollo, el código de la aplicación debe acceder a los secretos almacenados en las variables de entorno de manera segura.
3. **Despliegue en producción**: Al desplegar la aplicación en un entorno de producción, los secretos deben ser gestionados de manera que no se expongan a posibles atacantes.

### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD;
    A[Inicio] --> B[Instalar django-environ]
    B --> C[Configurar variables de entorno]
    C --> D[Modificar settings.py]
    D --> E[Acceso a secretos en el código]
    E --> F[Despliegue en producción]
    F --> G[Fin]
```

### d. Requisitos no funcionales

1. **Seguridad**: Los secretos deben ser manejados de manera que no se expongan en el código fuente o en repositorios de control de versiones.
2. **Escalabilidad**: La solución debe ser escalable para manejar múltiples entornos (desarrollo, pruebas, producción) con diferentes configuraciones de secretos.
3. **Mantenibilidad**: La configuración debe ser fácil de mantener y actualizar conforme cambien los secretos o se agreguen nuevos.

## Implementación en Python

### a. Explicación paso a paso del código

1. **Instalación de django-environ**:
   Primero, debemos instalar la librería `django-environ` en nuestro entorno de desarrollo. Esto se puede hacer utilizando `pip`.

2. **Configuración de variables de entorno**:
   Creamos un archivo `.env` en la raíz de nuestro proyecto para almacenar nuestras variables de entorno de manera segura.

3. **Modificación del archivo settings.py**:
   Modificamos el archivo `settings.py` de Django para utilizar las variables de entorno definidas en el archivo `.env`.

4. **Acceso a secretos en el código**:
   Mostramos cómo acceder a las variables de entorno en el código de nuestra aplicación.

### b. Código fuente completo y comentado

#### Instalación de django-environ

```bash
pip install django-environ
```

#### Configuración de variables de entorno

Creamos un archivo `.env` en la raíz de nuestro proyecto con el siguiente contenido:

```env
DEBUG=True
SECRET_KEY=supersecretkey
DATABASE_URL=postgres://user:password@localhost:5432/mydatabase
```

#### Modificación del archivo settings.py

```python
import environ

# Inicializar la instancia de django-environ
env = environ.Env(
    # Establecer valores por defecto
    DEBUG=(bool, False)
)

# Leer el archivo .env
environ.Env.read_env()

# Configurar las variables de entorno
DEBUG = env('DEBUG')
SECRET_KEY = env('SECRET_KEY')
DATABASES = {
    'default': env.db()
}
```

#### Acceso a secretos en el código

```python
# Ejemplo de acceso a una variable de entorno en el código
import os
from django.conf import settings

def get_secret_key():
    return settings.SECRET_KEY

print(get_secret_key())
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

```python
# views.py
from django.http import HttpResponse
from django.conf import settings

def secret_view(request):
    secret_key = settings.SECRET_KEY
    return HttpResponse(f"La clave secreta es: {secret_key}")
```

#### Pruebas unitarias

```python
# tests.py
from django.test import TestCase
from django.conf import settings

class SecretKeyTest(TestCase):
    def test_secret_key(self):
        self.assertEqual(settings.SECRET_KEY, 'supersecretkey')
```

## Mejores prácticas y consideraciones de diseño

1. **No almacenar secretos en el código fuente**: Nunca almacenes secretos directamente en el código fuente o en repositorios de control de versiones.
2. **Utilizar variables de entorno**: Utiliza variables de entorno para gestionar secretos de manera segura.
3. **Rotación de secretos**: Implementa un proceso para rotar secretos periódicamente y actualizar las configuraciones correspondientes.
4. **Acceso mínimo**: Limita el acceso a los secretos solo a aquellos componentes que realmente los necesitan.
5. **Monitoreo y auditoría**: Monitorea el acceso a los secretos y realiza auditorías periódicas para detectar posibles vulnerabilidades.

### Consideraciones sobre seguridad, rendimiento y escalabilidad

1. **Seguridad**: Asegúrate de que el archivo `.env` no esté accesible públicamente y que esté incluido en el archivo `.gitignore` para evitar su inclusión en repositorios de control de versiones.
2. **Rendimiento**: El uso de variables de entorno no debería impactar significativamente el rendimiento de la aplicación, pero es importante asegurar que la lectura de estas variables sea eficiente.
3. **Escalabilidad**: La solución debe ser capaz de manejar múltiples entornos con diferentes configuraciones de secretos sin necesidad de cambios significativos en el código.

### Consejos para la depuración y solución de problemas comunes

1. **Archivo .env no encontrado**: Asegúrate de que el archivo `.env` esté en la raíz del proyecto y que `environ.Env.read_env()` esté correctamente configurado en `settings.py`.
2. **Variables de entorno no cargadas**: Verifica que las variables de entorno estén correctamente definidas en el archivo `.env` y que no haya errores tipográficos.
3. **Acceso a secretos en producción**: Asegúrate de que las variables de entorno estén correctamente configuradas en el entorno de producción y que el archivo `.env` no esté expuesto públicamente.

Con esto, hemos cubierto de manera exhaustiva el manejo seguro de secretos utilizando `django-environ` en un sistema de carrito de compras con Django. Esta unidad proporciona una base sólida para gestionar secretos de manera segura y eficiente, protegiendo así nuestra aplicación y los datos sensibles que maneja.