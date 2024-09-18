# Unidad: Implementación de tokens JWT para la API

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la implementación de tokens JWT (JSON Web Tokens) para la autenticación y autorización en una API desarrollada con Django. Los tokens JWT son una forma segura y eficiente de manejar la autenticación en aplicaciones web modernas, permitiendo que los usuarios se autentiquen una vez y luego utilicen el token generado para acceder a recursos protegidos.

### Objetivos de aprendizaje

Al finalizar esta unidad, los estudiantes serán capaces de:

1. Comprender el concepto y la estructura de los tokens JWT.
2. Implementar la generación y validación de tokens JWT en una API Django.
3. Configurar Django para utilizar tokens JWT en lugar de sesiones tradicionales.
4. Asegurar rutas y recursos en la API utilizando tokens JWT.
5. Manejar la expiración y renovación de tokens JWT.
6. Aplicar mejores prácticas y consideraciones de seguridad al trabajar con tokens JWT.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es implementar un sistema de autenticación basado en tokens JWT para una API Django. Esto incluye la generación de tokens JWT al momento del login, la validación de estos tokens en cada solicitud a recursos protegidos, y la renovación de tokens cuando sea necesario.

### b. Casos de uso

1. **Generación de token JWT al iniciar sesión**:
   - **Actor**: Usuario
   - **Descripción**: El usuario envía sus credenciales (nombre de usuario y contraseña) a la API. Si las credenciales son correctas, la API genera un token JWT y lo devuelve al usuario.
   - **Precondiciones**: El usuario debe estar registrado en el sistema.
   - **Postcondiciones**: El usuario recibe un token JWT válido.

2. **Acceso a recursos protegidos**:
   - **Actor**: Usuario autenticado
   - **Descripción**: El usuario envía una solicitud a un recurso protegido de la API, incluyendo el token JWT en los encabezados de la solicitud. La API valida el token y, si es válido, permite el acceso al recurso.
   - **Precondiciones**: El usuario debe haber iniciado sesión y tener un token JWT válido.
   - **Postcondiciones**: El usuario accede al recurso protegido.

3. **Renovación de token JWT**:
   - **Actor**: Usuario autenticado
   - **Descripción**: El usuario envía una solicitud para renovar su token JWT antes de que expire. La API genera un nuevo token y lo devuelve al usuario.
   - **Precondiciones**: El usuario debe tener un token JWT válido que esté próximo a expirar.
   - **Postcondiciones**: El usuario recibe un nuevo token JWT con una nueva fecha de expiración.

### c. Diagramas de flujo (en mermaid)

```mermaid
flowchart TD
    A[Inicio de sesión] --> B[Usuario envía credenciales]
    B --> C{Credenciales válidas?}
    C -->|Sí| D[Generar token JWT]
    D --> E[Devolver token JWT]
    C -->|No| F[Devolver error]

    G[Acceso a recurso protegido] --> H[Usuario envía solicitud con token JWT]
    H --> I{Token válido?}
    I -->|Sí| J[Permitir acceso al recurso]
    I -->|No| K[Devolver error]

    L[Renovación de token] --> M[Usuario envía solicitud de renovación]
    M --> N{Token próximo a expirar?}
    N -->|Sí| O[Generar nuevo token JWT]
    O --> P[Devolver nuevo token JWT]
    N -->|No| Q[Devolver error]
```

### d. Requisitos no funcionales

1. **Seguridad**: Los tokens JWT deben ser firmados utilizando un algoritmo seguro (por ejemplo, HS256 o RS256) y una clave secreta robusta. Los tokens deben incluir una fecha de expiración para limitar su validez en el tiempo.
2. **Rendimiento**: La generación y validación de tokens JWT debe ser eficiente para no afectar el rendimiento general de la API.
3. **Escalabilidad**: El sistema debe ser capaz de manejar un gran número de solicitudes simultáneas, asegurando que la validación de tokens JWT no se convierta en un cuello de botella.
4. **Mantenibilidad**: El código debe ser modular y bien documentado, facilitando futuras modificaciones y extensiones.

## Implementación en Python

### a. Explicación paso a paso del código

Para implementar la autenticación basada en tokens JWT en Django, seguiremos los siguientes pasos:

1. **Instalación de dependencias**: Utilizaremos la librería `djangorestframework-simplejwt` para manejar los tokens JWT en Django.
2. **Configuración de Django**: Configuraremos Django para utilizar `djangorestframework-simplejwt` para la autenticación.
3. **Generación de tokens JWT**: Implementaremos la generación de tokens JWT al momento del login.
4. **Protección de rutas**: Configuraremos las rutas de la API para que requieran un token JWT válido.
5. **Renovación de tokens JWT**: Implementaremos la funcionalidad para renovar tokens JWT.

### b. Código fuente completo y comentado

#### Instalación de dependencias

Primero, instalamos la librería `djangorestframework-simplejwt`:

```sh
pip install djangorestframework-simplejwt
```

#### Configuración de Django

En el archivo `settings.py`, añadimos la configuración necesaria para utilizar `djangorestframework-simplejwt`:

```python
# settings.py

INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework_simplejwt',
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}

from datetime import timedelta

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=5),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=1),
    'ROTATE_REFRESH_TOKENS': True,
    'BLACKLIST_AFTER_ROTATION': True,
    'ALGORITHM': 'HS256',
    'SIGNING_KEY': SECRET_KEY,
    'VERIFYING_KEY': None,
    'AUTH_HEADER_TYPES': ('Bearer',),
    'USER_ID_FIELD': 'id',
    'USER_ID_CLAIM': 'user_id',
    'AUTH_TOKEN_CLASSES': ('rest_framework_simplejwt.tokens.AccessToken',),
    'TOKEN_TYPE_CLAIM': 'token_type',
}
```

#### Generación de tokens JWT

Creamos una vista para manejar el login y la generación de tokens JWT:

```python
# views.py

from rest_framework_simplejwt.tokens import RefreshToken
from rest_framework.response import Response
from rest_framework.decorators import api_view
from django.contrib.auth.models import User
from django.contrib.auth import authenticate

@api_view(['POST'])
def login(request):
    username = request.data.get('username')
    password = request.data.get('password')
    user = authenticate(username=username, password=password)
    if user is not None:
        refresh = RefreshToken.for_user(user)
        return Response({
            'refresh': str(refresh),
            'access': str(refresh.access_token),
        })
    else:
        return Response({'error': 'Credenciales inválidas'}, status=400)
```

#### Protección de rutas

Protegemos las rutas de la API utilizando el decorador `@permission_classes`:

```python
# views.py

from rest_framework.permissions import IsAuthenticated

@api_view(['GET'])
@permission_classes([IsAuthenticated])
def protected_view(request):
    return Response({'message': 'Este es un recurso protegido'})
```

#### Renovación de tokens JWT

Añadimos una vista para manejar la renovación de tokens JWT:

```python
# views.py

from rest_framework_simplejwt.views import TokenRefreshView

@api_view(['POST'])
def refresh_token(request):
    return TokenRefreshView.as_view()(request._request)
```

#### URLs

Configuramos las rutas en `urls.py`:

```python
# urls.py

from django.urls import path
from .views import login, protected_view, refresh_token

urlpatterns = [
    path('api/login/', login, name='login'),
    path('api/protected/', protected_view, name='protected'),
    path('api/token/refresh/', refresh_token, name='token_refresh'),
]
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

1. **Inicio de sesión y obtención de token JWT**:

```sh
curl -X POST http://localhost:8000/api/login/ -d "username=user&password=pass"
```

Respuesta:

```json
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

2. **Acceso a recurso protegido**:

```sh
curl -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..." http://localhost:8000/api/protected/
```

Respuesta:

```json
{
    "message": "Este es un recurso protegido"
}
```

3. **Renovación de token JWT**:

```sh
curl -X POST http://localhost:8000/api/token/refresh/ -d "refresh=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
```

Respuesta:

```json
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

#### Pruebas unitarias

Implementamos pruebas unitarias para verificar la funcionalidad de la autenticación basada en tokens JWT:

```python
# tests.py

from rest_framework.test import APITestCase
from django.contrib.auth.models import User
from rest_framework_simplejwt.tokens import RefreshToken

class JWTAuthenticationTests(APITestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='testuser', password='testpass')

    def test_login(self):
        response = self.client.post('/api/login/', {'username': 'testuser', 'password': 'testpass'})
        self.assertEqual(response.status_code, 200)
        self.assertIn('access', response.data)
        self.assertIn('refresh', response.data)

    def test_protected_view(self):
        refresh = RefreshToken.for_user(self.user)
        access = str(refresh.access_token)
        self.client.credentials(HTTP_AUTHORIZATION='Bearer ' + access)
        response = self.client.get('/api/protected/')
        self.assertEqual(response.status_code, 200)
        self.assertEqual(response.data, {'message': 'Este es un recurso protegido'})

    def test_token_refresh(self):
        refresh = RefreshToken.for_user(self.user)
        response = self.client.post('/api/token/refresh/', {'refresh': str(refresh)})
        self.assertEqual(response.status_code, 200)
        self.assertIn('access', response.data)
```

## Mejores prácticas y consideraciones de diseño

### Seguridad

1. **Uso de claves secretas robustas**: Asegúrese de que la clave secreta utilizada para firmar los tokens JWT sea lo suficientemente compleja y segura.
2. **Expiración de tokens**: Configure tiempos de expiración adecuados para los tokens JWT para minimizar el riesgo de uso indebido en caso de que un token sea comprometido.
3. **Almacenamiento seguro de tokens**: Almacene los tokens JWT de manera segura en el cliente, preferiblemente en `localStorage` o `sessionStorage` en aplicaciones web, y en el almacenamiento seguro en aplicaciones móviles.

### Rendimiento

1. **Validación eficiente**: La validación de tokens JWT debe ser rápida y eficiente. Evite realizar consultas a la base de datos durante la validación del token, ya que los tokens JWT contienen toda la información necesaria para la autenticación.
2. **Carga de trabajo distribuida**: En aplicaciones de gran escala, considere el uso de un balanceador de carga para distribuir la carga de trabajo entre múltiples servidores.

### Escalabilidad

1. **Desacoplamiento de servicios**: Utilice una arquitectura de microservicios para desacoplar la autenticación de otros servicios de la API, permitiendo una mayor flexibilidad y escalabilidad.
2. **Cacheo de tokens**: Considere el uso de un sistema de cacheo para almacenar tokens JWT y reducir la carga en el servidor de autenticación.

### Mantenibilidad

1. **Modularidad del código**: Mantenga el código modular y bien documentado para facilitar futuras modificaciones y extensiones.
2. **Pruebas automatizadas**: Implemente pruebas unitarias y de integración para asegurar que la funcionalidad de autenticación basada en tokens JWT funcione correctamente y sea robusta ante cambios.

### Depuración y solución de problemas comunes

1. **Errores de autenticación**: Verifique que el token JWT sea válido y no haya expirado. Asegúrese de que el encabezado de autorización esté correctamente formateado.
2. **Problemas de configuración**: Revise la configuración de `djangorestframework-simplejwt` en `settings.py` para asegurarse de que esté correctamente configurada.
3. **Problemas de rendimiento**: Utilice herramientas de monitoreo y perfilado para identificar cuellos de botella en la validación de tokens JWT y optimizar el rendimiento.

Con esta unidad, hemos cubierto de manera exhaustiva la implementación de tokens JWT para la autenticación en una API Django. Al seguir estos pasos y aplicar las mejores prácticas, podrá crear un sistema de autenticación seguro, eficiente y escalable para su aplicación de carrito de compras.