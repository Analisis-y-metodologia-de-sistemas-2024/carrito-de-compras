# Visión General del Proyecto: Sistema de Carrito de Compras con Django

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en proporcionar una visión general del proyecto "Sistema de Carrito de Compras con Django". Este capítulo es fundamental para entender el alcance y los objetivos del sistema, así como para establecer una base sólida para el desarrollo de las funcionalidades específicas en las unidades posteriores. Al final de esta unidad, los lectores deberían ser capaces de:

1. Comprender la estructura general y los componentes principales del sistema de carrito de compras.
2. Identificar los casos de uso más relevantes y cómo se interrelacionan.
3. Interpretar diagramas de flujo que describen los procesos clave del sistema.
4. Reconocer los requisitos no funcionales que deben ser considerados durante el desarrollo.
5. Implementar una base de código inicial en Python utilizando Django.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El sistema de carrito de compras es una aplicación web que permite a los usuarios navegar por un catálogo de productos, agregar productos a un carrito de compras, modificar las cantidades de los productos en el carrito y proceder al pago. La funcionalidad principal del sistema incluye:

1. **Navegación de Productos**: Los usuarios pueden ver una lista de productos disponibles, filtrarlos por categoría, precio, popularidad, etc.
2. **Gestión del Carrito de Compras**: Los usuarios pueden agregar productos al carrito, eliminar productos del carrito y actualizar las cantidades de los productos en el carrito.
3. **Proceso de Pago**: Los usuarios pueden proceder al pago, ingresar detalles de envío y pago, y confirmar la compra.
4. **Gestión de Usuarios**: Registro, inicio de sesión y gestión de perfiles de usuario.
5. **Administración del Sistema**: Los administradores pueden agregar, modificar y eliminar productos, así como gestionar pedidos y usuarios.

### Casos de Uso

#### Caso de Uso 1: Navegación de Productos

**Descripción**: Un usuario navega por el catálogo de productos, aplicando filtros y ordenando los resultados según sus preferencias.

**Actores**: Usuario

**Flujo Principal**:
1. El usuario accede a la página principal.
2. El usuario selecciona una categoría de productos.
3. El sistema muestra una lista de productos de la categoría seleccionada.
4. El usuario aplica filtros y ordena los resultados.
5. El sistema actualiza la lista de productos según los filtros aplicados.

#### Caso de Uso 2: Gestión del Carrito de Compras

**Descripción**: Un usuario agrega productos al carrito, modifica las cantidades y elimina productos del carrito.

**Actores**: Usuario

**Flujo Principal**:
1. El usuario selecciona un producto de la lista.
2. El usuario agrega el producto al carrito.
3. El sistema actualiza el carrito de compras.
4. El usuario accede al carrito de compras.
5. El usuario modifica las cantidades o elimina productos del carrito.
6. El sistema actualiza el carrito de compras.

#### Caso de Uso 3: Proceso de Pago

**Descripción**: Un usuario procede al pago, ingresa detalles de envío y pago, y confirma la compra.

**Actores**: Usuario

**Flujo Principal**:
1. El usuario accede al carrito de compras.
2. El usuario procede al pago.
3. El usuario ingresa detalles de envío y pago.
4. El usuario confirma la compra.
5. El sistema procesa el pago y confirma la compra.

### Diagramas de Flujo

#### Diagrama de Flujo: Navegación de Productos

```mermaid
graph TD
    A[Inicio] --> B[Seleccionar Categoría]
    B --> C[Mostrar Lista de Productos]
    C --> D[Aplicar Filtros]
    D --> E[Ordenar Resultados]
    E --> F[Mostrar Lista Actualizada]
```

#### Diagrama de Flujo: Gestión del Carrito de Compras

```mermaid
graph TD
    A[Seleccionar Producto] --> B[Agregar al Carrito]
    B --> C[Actualizar Carrito]
    C --> D[Acceder al Carrito]
    D --> E[Modificar Cantidades]
    E --> F[Eliminar Productos]
    F --> G[Actualizar Carrito]
```

#### Diagrama de Flujo: Proceso de Pago

```mermaid
graph TD
    A[Acceder al Carrito] --> B[Proceder al Pago]
    B --> C[Ingresar Detalles de Envío y Pago]
    C --> D[Confirmar Compra]
    D --> E[Procesar Pago]
    E --> F[Confirmar Compra]
```

### Requisitos No Funcionales

1. **Rendimiento**: El sistema debe ser capaz de manejar hasta 1000 usuarios concurrentes sin degradación significativa del rendimiento.
2. **Seguridad**: Los datos de los usuarios y las transacciones deben ser protegidos mediante cifrado SSL y otras medidas de seguridad.
3. **Escalabilidad**: El sistema debe ser escalable para soportar un aumento en el número de usuarios y productos sin requerir cambios significativos en la arquitectura.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar, con tiempos de respuesta rápidos.
5. **Mantenibilidad**: El código debe ser modular y bien documentado para facilitar su mantenimiento y actualización.

## Implementación en Python

### Explicación Paso a Paso del Código

Para implementar un sistema de carrito de compras en Django, comenzaremos con la configuración básica del proyecto y la creación de los modelos necesarios. Django es un framework de alto nivel que facilita el desarrollo rápido de aplicaciones web seguras y mantenibles.

#### Paso 1: Configuración del Proyecto

Primero, instalaremos Django y crearemos un nuevo proyecto.

```bash
pip install django
django-admin startproject carrito_compras
cd carrito_compras
```

#### Paso 2: Creación de la Aplicación

Dentro del proyecto, crearemos una nueva aplicación llamada `tienda`.

```bash
python manage.py startapp tienda
```

#### Paso 3: Definición de Modelos

Definiremos los modelos para los productos y el carrito de compras en `tienda/models.py`.

```python
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=200)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    categoria = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre

class Carrito(models.Model):
    usuario = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    creado = models.DateTimeField(auto_now_add=True)

class ItemCarrito(models.Model):
    carrito = models.ForeignKey(Carrito, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()

    def __str__(self):
        return f'{self.cantidad} x {self.producto.nombre}'
```

#### Paso 4: Migraciones y Administración

Aplicaremos las migraciones y registraremos los modelos en el administrador de Django.

```bash
python manage.py makemigrations
python manage.py migrate
```

En `tienda/admin.py`:

```python
from django.contrib import admin
from .models import Producto, Carrito, ItemCarrito

admin.site.register(Producto)
admin.site.register(Carrito)
admin.site.register(ItemCarrito)
```

### Código Fuente Completo y Comentado

A continuación, se presenta el código fuente completo y comentado para la configuración inicial del sistema de carrito de compras.

```python
# tienda/models.py

from django.db import models

class Producto(models.Model):
    """
    Modelo que representa un producto en la tienda.
    """
    nombre = models.CharField(max_length=200)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    categoria = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre

class Carrito(models.Model):
    """
    Modelo que representa un carrito de compras.
    """
    usuario = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    creado = models.DateTimeField(auto_now_add=True)

class ItemCarrito(models.Model):
    """
    Modelo que representa un ítem en el carrito de compras.
    """
    carrito = models.ForeignKey(Carrito, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()

    def __str__(self):
        return f'{self.cantidad} x {self.producto.nombre}'
```

### Ejemplos de Uso y Pruebas Unitarias

Para verificar que nuestros modelos funcionan correctamente, podemos escribir algunas pruebas unitarias en `tienda/tests.py`.

```python
from django.test import TestCase
from .models import Producto, Carrito, ItemCarrito
from django.contrib.auth.models import User

class ModeloTests(TestCase):

    def setUp(self):
        self.usuario = User.objects.create_user(username='testuser', password='12345')
        self.producto = Producto.objects.create(
            nombre='Producto de Prueba',
            descripcion='Descripción del producto de prueba',
            precio=100.00,
            stock=10,
            categoria='Categoría de Prueba'
        )
        self.carrito = Carrito.objects.create(usuario=self.usuario)

    def test_crear_producto(self):
        self.assertEqual(self.producto.nombre, 'Producto de Prueba')

    def test_agregar_producto_al_carrito(self):
        item_carrito = ItemCarrito.objects.create(carrito=self.carrito, producto=self.producto, cantidad=2)
        self.assertEqual(item_carrito.cantidad, 2)
        self.assertEqual(item_carrito.producto.nombre, 'Producto de Prueba')
```

### Mejores Prácticas y Consideraciones de Diseño

1. **Modularidad**: Mantener el código modular facilita su mantenimiento y escalabilidad. Dividir las funcionalidades en aplicaciones separadas dentro del proyecto Django puede ayudar a mantener el código organizado.
2. **Seguridad**: Asegurarse de que todas las transacciones y datos sensibles estén protegidos mediante cifrado SSL. Utilizar las herramientas de autenticación y autorización de Django para proteger las rutas y datos.
3. **Rendimiento**: Utilizar técnicas de caché y optimización de consultas para mejorar el rendimiento del sistema. Django ofrece varias opciones para caché que pueden ser útiles.
4. **Pruebas**: Escribir pruebas unitarias y de integración para asegurar que todas las funcionalidades del sistema funcionan correctamente. Django proporciona un framework de pruebas robusto que facilita esta tarea.
5. **Documentación**: Mantener una buena documentación del código y de las funcionalidades del sistema. Esto no solo ayuda a los desarrolladores actuales, sino también a futuros desarrolladores que trabajen en el proyecto.

En resumen, esta unidad proporciona una visión general del sistema de carrito de compras con Django, desde la definición de los requisitos funcionales y no funcionales hasta la implementación inicial y las mejores prácticas de diseño. Con esta base, los desarrolladores pueden avanzar hacia la implementación de funcionalidades más específicas y complejas en las unidades siguientes.

# Unidad: Alcance del Proyecto Django

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, exploraremos el alcance del proyecto Django en el contexto del desarrollo de un sistema de carrito de compras. Este capítulo es fundamental para comprender las limitaciones y las capacidades del proyecto, así como para establecer expectativas claras sobre lo que se puede lograr con Django. Al finalizar esta unidad, los lectores serán capaces de:

1. Definir el alcance del proyecto Django en el desarrollo de un sistema de carrito de compras.
2. Identificar las funcionalidades clave que se implementarán.
3. Comprender los casos de uso y los flujos de trabajo principales.
4. Reconocer los requisitos no funcionales que impactan el proyecto.
5. Implementar un prototipo básico del sistema utilizando Django.

## Documento Funcional de Requerimientos

### a. Descripción Detallada de la Funcionalidad

El sistema de carrito de compras con Django tiene como objetivo proporcionar una plataforma en línea donde los usuarios puedan navegar por productos, añadirlos a un carrito de compras, y proceder a la compra. Las funcionalidades clave incluyen:

1. **Gestión de Usuarios**: Registro, inicio de sesión, y gestión de perfiles.
2. **Catálogo de Productos**: Visualización de productos, búsqueda y filtrado.
3. **Carrito de Compras**: Añadir, eliminar y actualizar productos en el carrito.
4. **Proceso de Pago**: Integración con pasarelas de pago para completar compras.
5. **Historial de Pedidos**: Visualización de pedidos anteriores y su estado.
6. **Administración**: Panel de administración para gestionar productos, usuarios y pedidos.

### b. Casos de Uso

#### Caso de Uso 1: Registro de Usuario

**Descripción**: Un nuevo usuario se registra en la plataforma proporcionando su información básica.

**Actor Principal**: Usuario

**Flujo Principal**:
1. El usuario accede a la página de registro.
2. El usuario completa el formulario de registro con su información.
3. El sistema valida la información y crea una nueva cuenta.
4. El usuario recibe una confirmación de registro.

#### Caso de Uso 2: Añadir Producto al Carrito

**Descripción**: Un usuario añade un producto a su carrito de compras.

**Actor Principal**: Usuario

**Flujo Principal**:
1. El usuario navega por el catálogo de productos.
2. El usuario selecciona un producto y hace clic en "Añadir al carrito".
3. El sistema añade el producto al carrito del usuario.
4. El usuario recibe una notificación de que el producto ha sido añadido.

#### Caso de Uso 3: Proceso de Pago

**Descripción**: Un usuario completa el proceso de pago para los productos en su carrito.

**Actor Principal**: Usuario

**Flujo Principal**:
1. El usuario revisa su carrito y procede al pago.
2. El usuario ingresa la información de pago.
3. El sistema valida la información y procesa el pago.
4. El usuario recibe una confirmación de compra.

### c. Diagramas de Flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Registro de Usuario]
    B --> C[Validación de Datos]
    C --> D{Datos válidos?}
    D -- Sí --> E[Crear Cuenta]
    D -- No --> F[Mostrar Error]
    E --> G[Confirmación de Registro]
    G --> H[Fin]

    subgraph Proceso de Registro
        B --> C --> D --> E --> G
    end

    A --> I[Navegar Catálogo]
    I --> J[Seleccionar Producto]
    J --> K[Añadir al Carrito]
    K --> L[Confirmación de Añadido]

    subgraph Añadir Producto al Carrito
        I --> J --> K --> L
    end

    A --> M[Revisar Carrito]
    M --> N[Proceder al Pago]
    N --> O[Ingresar Información de Pago]
    O --> P[Validación de Pago]
    P --> Q{Pago Válido?}
    Q -- Sí --> R[Procesar Pago]
    Q -- No --> S[Mostrar Error]
    R --> T[Confirmación de Compra]

    subgraph Proceso de Pago
        M --> N --> O --> P --> Q --> R --> T
    end
```

### d. Requisitos No Funcionales

1. **Seguridad**: El sistema debe proteger la información del usuario y las transacciones financieras mediante encriptación y autenticación segura.
2. **Rendimiento**: El sistema debe ser capaz de manejar múltiples usuarios simultáneamente sin degradar el rendimiento.
3. **Escalabilidad**: El sistema debe ser escalable para soportar un aumento en el número de usuarios y productos.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar.
5. **Mantenibilidad**: El código debe ser modular y bien documentado para facilitar el mantenimiento y la actualización.

## Implementación en Python

### a. Explicación Paso a Paso del Código

Para implementar el sistema de carrito de compras, utilizaremos el framework Django debido a su robustez y facilidad de uso. A continuación, se presenta una implementación básica de algunas de las funcionalidades descritas.

### b. Código Fuente Completo y Comentado

#### Configuración Inicial

Primero, instalamos Django y creamos un nuevo proyecto:

```bash
pip install django
django-admin startproject carrito_compras
cd carrito_compras
django-admin startapp tienda
```

#### Modelos

Definimos los modelos para los productos y el carrito de compras en `tienda/models.py`:

```python
from django.db import models
from django.contrib.auth.models import User

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()

    def __str__(self):
        return self.nombre

class Carrito(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE)
    productos = models.ManyToManyField(Producto, through='CarritoProducto')

class CarritoProducto(models.Model):
    carrito = models.ForeignKey(Carrito, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()

    def __str__(self):
        return f'{self.producto.nombre} en {self.carrito.usuario.username}'
```

#### Vistas

Creamos vistas para manejar las operaciones del carrito en `tienda/views.py`:

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Producto, Carrito, CarritoProducto
from django.contrib.auth.decorators import login_required

@login_required
def agregar_al_carrito(request, producto_id):
    producto = get_object_or_404(Producto, id=producto_id)
    carrito, created = Carrito.objects.get_or_create(usuario=request.user)
    carrito_producto, created = CarritoProducto.objects.get_or_create(carrito=carrito, producto=producto)
    if not created:
        carrito_producto.cantidad += 1
    carrito_producto.save()
    return redirect('ver_carrito')

@login_required
def ver_carrito(request):
    carrito, created = Carrito.objects.get_or_create(usuario=request.user)
    return render(request, 'tienda/ver_carrito.html', {'carrito': carrito})
```

#### Plantillas

Creamos plantillas para mostrar el carrito en `tienda/templates/tienda/ver_carrito.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Carrito de Compras</title>
</head>
<body>
    <h1>Carrito de Compras</h1>
    <ul>
        {% for item in carrito.carritoproducto_set.all %}
            <li>{{ item.producto.nombre }} - Cantidad: {{ item.cantidad }}</li>
        {% endfor %}
    </ul>
    <a href="{% url 'catalogo' %}">Seguir comprando</a>
</body>
</html>
```

### c. Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

Para probar la funcionalidad, primero debemos crear algunos productos desde el panel de administración de Django. Luego, un usuario registrado puede añadir productos a su carrito y verlos.

#### Pruebas Unitarias

Creamos pruebas unitarias para verificar la funcionalidad en `tienda/tests.py`:

```python
from django.test import TestCase
from django.contrib.auth.models import User
from .models import Producto, Carrito, CarritoProducto

class CarritoTests(TestCase):

    def setUp(self):
        self.usuario = User.objects.create_user(username='testuser', password='12345')
        self.producto = Producto.objects.create(nombre='Producto 1', descripcion='Descripción del producto 1', precio=100.00, stock=10)

    def test_agregar_al_carrito(self):
        self.client.login(username='testuser', password='12345')
        response = self.client.get(f'/agregar_al_carrito/{self.producto.id}/')
        self.assertEqual(response.status_code, 302)
        carrito = Carrito.objects.get(usuario=self.usuario)
        self.assertEqual(carrito.carritoproducto_set.count(), 1)
        self.assertEqual(carrito.carritoproducto_set.first().producto, self.producto)
```

## Mejores Prácticas y Consideraciones de Diseño

### Seguridad

1. **Autenticación y Autorización**: Utilizar el sistema de autenticación de Django para proteger las rutas sensibles.
2. **Encriptación**: Asegurarse de que todas las transacciones financieras estén encriptadas utilizando HTTPS.

### Rendimiento

1. **Optimización de Consultas**: Utilizar `select_related` y `prefetch_related` para optimizar las consultas a la base de datos.
2. **Caching**: Implementar caching para reducir la carga en la base de datos y mejorar el tiempo de respuesta.

### Escalabilidad

1. **Despliegue en la Nube**: Considerar el uso de servicios en la nube como AWS o Heroku para escalar la aplicación según la demanda.
2. **Microservicios**: Dividir la aplicación en microservicios para mejorar la escalabilidad y la mantenibilidad.

### Usabilidad

1. **Interfaz Intuitiva**: Diseñar una interfaz de usuario que sea fácil de navegar y utilizar.
2. **Accesibilidad**: Asegurarse de que la aplicación sea accesible para usuarios con discapacidades.

### Mantenibilidad

1. **Código Modular**: Escribir código modular y reutilizable.
2. **Documentación**: Documentar el código y las funcionalidades de manera exhaustiva para facilitar futuras actualizaciones y mantenimiento.

### Depuración y Solución de Problemas Comunes

1. **Logs**: Utilizar logs para rastrear y solucionar errores.
2. **Pruebas**: Implementar pruebas unitarias y de integración para asegurar que el sistema funcione correctamente.
3. **Herramientas de Depuración**: Utilizar herramientas de depuración como `pdb` y Django Debug Toolbar para identificar y solucionar problemas.

En resumen, el alcance del proyecto Django para un sistema de carrito de compras abarca una amplia gama de funcionalidades y consideraciones técnicas. Siguiendo las mejores prácticas y utilizando las herramientas adecuadas, es posible desarrollar una aplicación robusta, segura y escalable que cumpla con las expectativas de los usuarios.

# Unidad: Stakeholders y Usuarios Finales

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en identificar y comprender a los stakeholders y usuarios finales de un sistema de carrito de compras desarrollado con Django. Entender quiénes son las partes interesadas y los usuarios finales es crucial para el éxito del proyecto, ya que sus necesidades y expectativas deben ser consideradas durante todo el ciclo de vida del desarrollo. Al finalizar esta unidad, deberías ser capaz de:

1. Identificar y categorizar a los stakeholders y usuarios finales.
2. Comprender sus roles y expectativas.
3. Integrar sus necesidades en el diseño y desarrollo del sistema.
4. Crear un documento funcional de requerimientos que refleje estas necesidades.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El sistema de carrito de compras es una aplicación web que permite a los usuarios navegar por un catálogo de productos, agregar productos a un carrito, y proceder al pago. Para asegurar que el sistema cumpla con las expectativas de todos los stakeholders y usuarios finales, es esencial definir claramente quiénes son y qué esperan del sistema.

#### Stakeholders

1. **Propietario del Negocio**: Persona o entidad que posee la tienda en línea. Su interés principal es maximizar las ventas y la satisfacción del cliente.
2. **Desarrolladores**: Equipo encargado de construir y mantener el sistema. Su objetivo es crear una aplicación funcional, escalable y segura.
3. **Equipo de Marketing**: Encargados de atraer tráfico al sitio y convertir visitantes en clientes. Necesitan herramientas para analizar el comportamiento del usuario y ejecutar campañas de marketing.
4. **Equipo de Soporte al Cliente**: Personal que asiste a los clientes con problemas o consultas. Necesitan acceso a información del cliente y herramientas para resolver problemas rápidamente.
5. **Proveedores**: Entidades que suministran los productos vendidos en la tienda. Necesitan información sobre inventarios y ventas.

#### Usuarios Finales

1. **Clientes Registrados**: Usuarios que han creado una cuenta en la tienda. Pueden acceder a funcionalidades adicionales como historial de compras y listas de deseos.
2. **Clientes Invitados**: Usuarios que realizan compras sin registrarse. Necesitan un proceso de compra rápido y sencillo.
3. **Administradores del Sistema**: Usuarios con permisos para gestionar el catálogo de productos, usuarios y pedidos. Necesitan interfaces intuitivas y seguras para realizar sus tareas.

### Casos de Uso

#### Caso de Uso 1: Navegación y Búsqueda de Productos

**Descripción**: Un cliente puede navegar por las categorías de productos y buscar productos específicos utilizando una barra de búsqueda.

**Actores**: Clientes Registrados, Clientes Invitados

**Flujo Principal**:
1. El usuario accede a la página principal.
2. El usuario selecciona una categoría de productos o utiliza la barra de búsqueda.
3. El sistema muestra una lista de productos que coinciden con la categoría o términos de búsqueda.
4. El usuario selecciona un producto para ver más detalles.

**Flujo Alternativo**:
- Si no se encuentran productos que coincidan con los términos de búsqueda, el sistema muestra un mensaje indicando que no hay resultados.

#### Caso de Uso 2: Agregar Productos al Carrito

**Descripción**: Un cliente puede agregar productos a su carrito de compras.

**Actores**: Clientes Registrados, Clientes Invitados

**Flujo Principal**:
1. El usuario visualiza los detalles de un producto.
2. El usuario selecciona la cantidad deseada y hace clic en "Agregar al Carrito".
3. El sistema actualiza el carrito de compras del usuario.

**Flujo Alternativo**:
- Si el producto no está disponible en la cantidad deseada, el sistema muestra un mensaje indicando la disponibilidad.

### Diagramas de Flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Navegar/Búsqueda de Productos]
    B --> C[Seleccionar Producto]
    C --> D[Ver Detalles del Producto]
    D --> E[Agregar al Carrito]
    E --> F[Actualizar Carrito]
    F --> G[Continuar Comprando]
    F --> H[Proceder al Pago]
```

### Requisitos No Funcionales

1. **Seguridad**: El sistema debe proteger la información personal y de pago de los usuarios mediante cifrado y otras medidas de seguridad.
2. **Rendimiento**: El sistema debe ser capaz de manejar múltiples usuarios simultáneamente sin degradar el rendimiento.
3. **Escalabilidad**: El sistema debe ser escalable para soportar un crecimiento en el número de usuarios y productos.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar.
5. **Mantenibilidad**: El código debe ser limpio y bien documentado para facilitar el mantenimiento y las actualizaciones.

## Implementación en Python

### Explicación Paso a Paso del Código

Para implementar el sistema de carrito de compras en Django, seguiremos estos pasos:

1. **Configuración del Proyecto**: Crear un nuevo proyecto Django y configurar las aplicaciones necesarias.
2. **Modelado de Datos**: Definir los modelos para productos, carritos y pedidos.
3. **Vistas y URLs**: Crear vistas para la navegación, búsqueda, y gestión del carrito.
4. **Plantillas**: Diseñar las plantillas HTML para las páginas del sistema.
5. **Autenticación y Autorización**: Implementar el sistema de registro y login de usuarios.
6. **Pruebas Unitarias**: Escribir pruebas para asegurar que el sistema funciona correctamente.

### Código Fuente Completo y Comentado

#### Configuración del Proyecto

```python
# Crear un nuevo proyecto Django
django-admin startproject carrito_compras

# Navegar al directorio del proyecto
cd carrito_compras

# Crear una nueva aplicación para la tienda
python manage.py startapp tienda
```

#### Modelado de Datos

```python
# tienda/models.py
from django.db import models
from django.contrib.auth.models import User

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    imagen = models.ImageField(upload_to='productos/')

    def __str__(self):
        return self.nombre

class Carrito(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE)
    productos = models.ManyToManyField(Producto, through='CarritoProducto')

class CarritoProducto(models.Model):
    carrito = models.ForeignKey(Carrito, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()

class Pedido(models.Model):
    usuario = models.ForeignKey(User, on_delete=models.CASCADE)
    productos = models.ManyToManyField(Producto, through='PedidoProducto')
    fecha = models.DateTimeField(auto_now_add=True)
    total = models.DecimalField(max_digits=10, decimal_places=2)

class PedidoProducto(models.Model):
    pedido = models.ForeignKey(Pedido, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()
```

#### Vistas y URLs

```python
# tienda/views.py
from django.shortcuts import render, get_object_or_404
from .models import Producto, Carrito, CarritoProducto

def lista_productos(request):
    productos = Producto.objects.all()
    return render(request, 'tienda/lista_productos.html', {'productos': productos})

def detalle_producto(request, producto_id):
    producto = get_object_or_404(Producto, id=producto_id)
    return render(request, 'tienda/detalle_producto.html', {'producto': producto})

def agregar_al_carrito(request, producto_id):
    producto = get_object_or_404(Producto, id=producto_id)
    carrito, created = Carrito.objects.get_or_create(usuario=request.user)
    carrito_producto, created = CarritoProducto.objects.get_or_create(carrito=carrito, producto=producto)
    carrito_producto.cantidad += 1
    carrito_producto.save()
    return redirect('detalle_producto', producto_id=producto.id)

# tienda/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.lista_productos, name='lista_productos'),
    path('producto/<int:producto_id>/', views.detalle_producto, name='detalle_producto'),
    path('agregar/<int:producto_id>/', views.agregar_al_carrito, name='agregar_al_carrito'),
]
```

#### Plantillas

```html
<!-- tienda/templates/tienda/lista_productos.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Lista de Productos</title>
</head>
<body>
    <h1>Productos</h1>
    <ul>
        {% for producto in productos %}
            <li>
                <a href="{% url 'detalle_producto' producto.id %}">{{ producto.nombre }}</a>
                - ${{ producto.precio }}
            </li>
        {% endfor %}
    </ul>
</body>
</html>
```

```html
<!-- tienda/templates/tienda/detalle_producto.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{{ producto.nombre }}</title>
</head>
<body>
    <h1>{{ producto.nombre }}</h1>
    <p>{{ producto.descripcion }}</p>
    <p>Precio: ${{ producto.precio }}</p>
    <p>Stock: {{ producto.stock }}</p>
    <form action="{% url 'agregar_al_carrito' producto.id %}" method="post">
        {% csrf_token %}
        <button type="submit">Agregar al Carrito</button>
    </form>
</body>
</html>
```

#### Autenticación y Autorización

```python
# tienda/views.py (continuación)
from django.contrib.auth.decorators import login_required

@login_required
def agregar_al_carrito(request, producto_id):
    producto = get_object_or_404(Producto, id=producto_id)
    carrito, created = Carrito.objects.get_or_create(usuario=request.user)
    carrito_producto, created = CarritoProducto.objects.get_or_create(carrito=carrito, producto=producto)
    carrito_producto.cantidad += 1
    carrito_producto.save()
    return redirect('detalle_producto', producto_id=producto.id)
```

#### Pruebas Unitarias

```python
# tienda/tests.py
from django.test import TestCase
from django.contrib.auth.models import User
from .models import Producto, Carrito, CarritoProducto

class CarritoTestCase(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='testuser', password='12345')
        self.producto = Producto.objects.create(nombre='Producto 1', descripcion='Descripción del producto 1', precio=10.00, stock=100)

    def test_agregar_al_carrito(self):
        self.client.login(username='testuser', password='12345')
        response = self.client.post(f'/agregar/{self.producto.id}/')
        self.assertEqual(response.status_code, 302)
        carrito = Carrito.objects.get(usuario=self.user)
        carrito_producto = CarritoProducto.objects.get(carrito=carrito, producto=self.producto)
        self.assertEqual(carrito_producto.cantidad, 1)
```

### Ejemplos de Uso y Pruebas Unitarias

Para probar el sistema, puedes ejecutar el servidor de desarrollo de Django y navegar a las URLs definidas. Asegúrate de crear algunos productos en la base de datos para probar la funcionalidad de navegación y agregar al carrito.

```bash
# Ejecutar el servidor de desarrollo
python manage.py runserver
```

Luego, abre un navegador web y navega a `http://localhost:8000/` para ver la lista de productos. Haz clic en un producto para ver sus detalles y agregarlo al carrito.

## Mejores Prácticas y Consideraciones de Diseño

1. **Modularidad**: Mantén el código modular separando las responsabilidades en diferentes aplicaciones dentro del proyecto Django.
2. **Seguridad**: Utiliza las herramientas de Django para manejar la autenticación y autorización de usuarios, y asegúrate de proteger las rutas sensibles con decoradores como `@login_required`.
3. **Pruebas**: Escribe pruebas unitarias y de integración para asegurar que todas las funcionalidades del sistema funcionan correctamente.
4. **Documentación**: Documenta el código y las decisiones de diseño para facilitar el mantenimiento y la colaboración con otros desarrolladores.
5. **Optimización**: Optimiza las consultas a la base de datos utilizando select_related y prefetch_related para mejorar el rendimiento.
6. **Escalabilidad**: Diseña el sistema pensando en la escalabilidad, utilizando técnicas como la paginación para manejar grandes volúmenes de datos.

En resumen, identificar y comprender a los stakeholders y usuarios finales es esencial para el éxito de un sistema de carrito de compras. Al seguir las mejores prácticas y considerar las necesidades de todas las partes interesadas, puedes crear una aplicación robusta y eficiente que satisfaga las expectativas de todos.

# Sistema de Carrito de Compras con Django

## Unidad: Tecnologías principales: Django, Django REST framework

### Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en las tecnologías principales que utilizaremos para el desarrollo del sistema de carrito de compras: Django y Django REST framework (DRF). Django es un framework de alto nivel para el desarrollo de aplicaciones web en Python, conocido por su simplicidad y rapidez en el desarrollo. Django REST framework, por otro lado, es una poderosa y flexible herramienta para construir APIs web. Al finalizar esta unidad, los lectores deberán ser capaces de:

1. Comprender las características y beneficios de Django y Django REST framework.
2. Configurar un entorno de desarrollo para Django y DRF.
3. Implementar funcionalidades básicas de un sistema de carrito de compras utilizando Django y DRF.
4. Aplicar mejores prácticas en el desarrollo con Django y DRF.

### Documento funcional de requerimientos

#### a. Descripción detallada de la funcionalidad

El sistema de carrito de compras que desarrollaremos permitirá a los usuarios realizar las siguientes acciones:

1. **Registro y autenticación de usuarios**: Los usuarios podrán registrarse, iniciar sesión y cerrar sesión en el sistema.
2. **Gestión de productos**: Los administradores podrán agregar, editar y eliminar productos del catálogo.
3. **Visualización de productos**: Los usuarios podrán ver una lista de productos disponibles y detalles de cada producto.
4. **Gestión del carrito de compras**: Los usuarios podrán agregar productos a su carrito, actualizar la cantidad de productos y eliminar productos del carrito.
5. **Realización de pedidos**: Los usuarios podrán realizar pedidos basados en los productos en su carrito de compras.

#### b. Casos de uso

1. **Registro de usuario**:
   - **Actor**: Usuario
   - **Descripción**: Un usuario se registra en el sistema proporcionando su información personal.
   - **Precondición**: El usuario no debe estar registrado previamente.
   - **Postcondición**: El usuario es registrado y puede iniciar sesión.

2. **Inicio de sesión**:
   - **Actor**: Usuario
   - **Descripción**: Un usuario inicia sesión en el sistema proporcionando sus credenciales.
   - **Precondición**: El usuario debe estar registrado.
   - **Postcondición**: El usuario es autenticado y puede acceder a las funcionalidades del sistema.

3. **Agregar producto al catálogo**:
   - **Actor**: Administrador
   - **Descripción**: Un administrador agrega un nuevo producto al catálogo.
   - **Precondición**: El administrador debe estar autenticado.
   - **Postcondición**: El producto es agregado al catálogo y está disponible para los usuarios.

4. **Agregar producto al carrito**:
   - **Actor**: Usuario
   - **Descripción**: Un usuario agrega un producto a su carrito de compras.
   - **Precondición**: El usuario debe estar autenticado.
   - **Postcondición**: El producto es agregado al carrito del usuario.

5. **Realizar pedido**:
   - **Actor**: Usuario
   - **Descripción**: Un usuario realiza un pedido basado en los productos en su carrito de compras.
   - **Precondición**: El usuario debe tener productos en su carrito.
   - **Postcondición**: El pedido es registrado en el sistema.

#### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Registro de usuario]
    B --> C[Inicio de sesión]
    C --> D[Agregar producto al catálogo]
    D --> E[Visualización de productos]
    E --> F[Agregar producto al carrito]
    F --> G[Realizar pedido]
    G --> H[Fin]
```

#### d. Requisitos no funcionales

1. **Rendimiento**: El sistema debe ser capaz de manejar al menos 1000 usuarios concurrentes sin degradación significativa del rendimiento.
2. **Seguridad**: El sistema debe proteger los datos de los usuarios mediante el uso de HTTPS y almacenamiento seguro de contraseñas.
3. **Escalabilidad**: El sistema debe ser escalable para soportar un crecimiento futuro en el número de usuarios y productos.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar para garantizar una buena experiencia de usuario.
5. **Mantenibilidad**: El código debe ser modular y seguir las mejores prácticas de desarrollo para facilitar el mantenimiento y la evolución del sistema.

### Implementación en Python

#### a. Explicación paso a paso del código

Para implementar el sistema de carrito de compras, seguiremos los siguientes pasos:

1. **Configuración del entorno de desarrollo**: Instalación de Django y Django REST framework.
2. **Creación del proyecto Django**: Inicialización del proyecto y configuración básica.
3. **Definición de modelos**: Creación de modelos para usuarios, productos, carritos y pedidos.
4. **Implementación de vistas y serializadores**: Creación de vistas y serializadores para manejar las operaciones CRUD.
5. **Configuración de URLs**: Definición de rutas para las diferentes funcionalidades del sistema.
6. **Autenticación y permisos**: Implementación de autenticación y permisos para asegurar el acceso a las funcionalidades.

#### b. Código fuente completo y comentado

##### 1. Configuración del entorno de desarrollo

Para comenzar, necesitamos instalar Django y Django REST framework. Esto se puede hacer utilizando `pip`:

```bash
pip install django djangorestframework
```

##### 2. Creación del proyecto Django

Iniciamos un nuevo proyecto Django:

```bash
django-admin startproject carrito_compras
cd carrito_compras
```

Luego, creamos una nueva aplicación dentro del proyecto:

```bash
python manage.py startapp tienda
```

##### 3. Definición de modelos

En el archivo `models.py` de la aplicación `tienda`, definimos los modelos para usuarios, productos, carritos y pedidos:

```python
from django.db import models
from django.contrib.auth.models import User

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()

    def __str__(self):
        return self.nombre

class Carrito(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE)
    productos = models.ManyToManyField(Producto, through='CarritoProducto')

class CarritoProducto(models.Model):
    carrito = models.ForeignKey(Carrito, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()

class Pedido(models.Model):
    usuario = models.ForeignKey(User, on_delete=models.CASCADE)
    productos = models.ManyToManyField(Producto, through='PedidoProducto')
    fecha = models.DateTimeField(auto_now_add=True)
    total = models.DecimalField(max_digits=10, decimal_places=2)

class PedidoProducto(models.Model):
    pedido = models.ForeignKey(Pedido, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()
```

##### 4. Implementación de vistas y serializadores

Creamos serializadores en un archivo `serializers.py`:

```python
from rest_framework import serializers
from .models import Producto, Carrito, Pedido

class ProductoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Producto
        fields = '__all__'

class CarritoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Carrito
        fields = '__all__'

class PedidoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Pedido
        fields = '__all__'
```

Luego, creamos vistas en el archivo `views.py`:

```python
from rest_framework import viewsets
from .models import Producto, Carrito, Pedido
from .serializers import ProductoSerializer, CarritoSerializer, PedidoSerializer

class ProductoViewSet(viewsets.ModelViewSet):
    queryset = Producto.objects.all()
    serializer_class = ProductoSerializer

class CarritoViewSet(viewsets.ModelViewSet):
    queryset = Carrito.objects.all()
    serializer_class = CarritoSerializer

class PedidoViewSet(viewsets.ModelViewSet):
    queryset = Pedido.objects.all()
    serializer_class = PedidoSerializer
```

##### 5. Configuración de URLs

En el archivo `urls.py` del proyecto, configuramos las rutas:

```python
from django.contrib import admin
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from tienda import views

router = DefaultRouter()
router.register(r'productos', views.ProductoViewSet)
router.register(r'carritos', views.CarritoViewSet)
router.register(r'pedidos', views.PedidoViewSet)

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include(router.urls)),
]
```

##### 6. Autenticación y permisos

Para manejar la autenticación, agregamos `rest_framework` y `rest_framework.authtoken` a `INSTALLED_APPS` en `settings.py` y configuramos los permisos:

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework.authtoken',
    'tienda',
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
}
```

Finalmente, creamos vistas para el registro e inicio de sesión en `views.py`:

```python
from rest_framework.authtoken.models import Token
from rest_framework.decorators import api_view
from rest_framework.response import Response
from django.contrib.auth.models import User
from django.contrib.auth import authenticate

@api_view(['POST'])
def registro(request):
    username = request.data.get('username')
    password = request.data.get('password')
    user = User.objects.create_user(username=username, password=password)
    token, created = Token.objects.get_or_create(user=user)
    return Response({'token': token.key})

@api_view(['POST'])
def inicio_sesion(request):
    username = request.data.get('username')
    password = request.data.get('password')
    user = authenticate(username=username, password=password)
    if user:
        token, created = Token.objects.get_or_create(user=user)
        return Response({'token': token.key})
    else:
        return Response({'error': 'Credenciales inválidas'}, status=400)
```

Actualizamos `urls.py` para incluir las rutas de registro e inicio de sesión:

```python
urlpatterns += [
    path('api/registro/', views.registro),
    path('api/inicio_sesion/', views.inicio_sesion),
]
```

#### c. Ejemplos de uso y pruebas unitarias

Para probar las funcionalidades, podemos utilizar herramientas como `curl` o Postman para realizar solicitudes HTTP a las rutas definidas.

Ejemplo de registro de usuario:

```bash
curl -X POST http://localhost:8000/api/registro/ -d "username=usuario&password=contraseña"
```

Ejemplo de inicio de sesión:

```bash
curl -X POST http://localhost:8000/api/inicio_sesion/ -d "username=usuario&password=contraseña"
```

Ejemplo de agregar un producto (requiere autenticación):

```bash
curl -X POST http://localhost:8000/api/productos/ -H "Authorization: Token <token>" -d "nombre=Producto1&descripcion=Descripción&precio=100.00&stock=10"
```

Para las pruebas unitarias, podemos utilizar el módulo `unittest` de Python. Creamos un archivo `tests.py` en la aplicación `tienda`:

```python
from django.test import TestCase
from django.contrib.auth.models import User
from .models import Producto, Carrito, Pedido

class ModeloTests(TestCase):

    def test_crear_producto(self):
        producto = Producto.objects.create(nombre='Producto1', descripcion='Descripción', precio=100.00, stock=10)
        self.assertEqual(producto.nombre, 'Producto1')

    def test_crear_carrito(self):
        usuario = User.objects.create_user(username='usuario', password='contraseña')
        carrito = Carrito.objects.create(usuario=usuario)
        self.assertEqual(carrito.usuario.username, 'usuario')

    def test_crear_pedido(self):
        usuario = User.objects.create_user(username='usuario', password='contraseña')
        pedido = Pedido.objects.create(usuario=usuario, total=100.00)
        self.assertEqual(pedido.usuario.username, 'usuario')
```

### Mejores prácticas y consideraciones de diseño

1. **Modularidad**: Mantener el código modular y organizado en diferentes aplicaciones dentro del proyecto Django.
2. **Seguridad**: Utilizar autenticación y permisos adecuados para proteger los datos de los usuarios.
3. **Validación de datos**: Implementar validaciones en los serializadores para garantizar la integridad de los datos.
4. **Pruebas**: Escribir pruebas unitarias y de integración para asegurar la calidad del código.
5. **Documentación**: Mantener una buena documentación del código y de las APIs para facilitar el mantenimiento y la colaboración.

En resumen, Django y Django REST framework proporcionan una base sólida para el desarrollo de un sistema de carrito de compras. Siguiendo las mejores prácticas y consideraciones de diseño, podemos construir una aplicación robusta, segura y escalable que cumpla con los requisitos funcionales y no funcionales definidos.

# Unidad: Cronograma y Fases del Proyecto

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, exploraremos en detalle el cronograma y las fases del proyecto para el desarrollo de un sistema de carrito de compras utilizando Django. El objetivo principal es proporcionar una comprensión clara y estructurada del proceso de desarrollo, desde la planificación inicial hasta la implementación y el despliegue final. Al finalizar esta unidad, los lectores deberán ser capaces de:

1. Comprender las diferentes fases del ciclo de vida del proyecto.
2. Elaborar un cronograma detallado que contemple todas las actividades y entregables del proyecto.
3. Identificar los hitos clave y los puntos de control.
4. Implementar técnicas de gestión de proyectos para asegurar el cumplimiento de plazos y objetivos.
5. Aplicar mejores prácticas en la planificación y ejecución de proyectos de desarrollo de software.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El cronograma y las fases del proyecto son fundamentales para garantizar que el desarrollo del sistema de carrito de compras se realice de manera organizada y eficiente. Estas fases incluyen la planificación, el análisis de requerimientos, el diseño, la implementación, las pruebas, el despliegue y el mantenimiento. Cada fase tiene sus propias actividades y entregables específicos, que deben ser gestionados y monitoreados cuidadosamente.

### Casos de Uso

#### Caso de Uso 1: Planificación del Proyecto

**Descripción:** Definir el alcance del proyecto, identificar los recursos necesarios y establecer un cronograma preliminar.

**Actores:** Gerente de Proyecto, Equipo de Desarrollo.

**Flujo Principal:**
1. El Gerente de Proyecto se reúne con los stakeholders para definir el alcance del proyecto.
2. Se identifican y asignan los recursos necesarios.
3. Se elabora un cronograma preliminar con las fases y actividades del proyecto.

#### Caso de Uso 2: Análisis de Requerimientos

**Descripción:** Recopilar y documentar los requisitos funcionales y no funcionales del sistema de carrito de compras.

**Actores:** Analista de Requerimientos, Stakeholders, Usuarios Finales.

**Flujo Principal:**
1. El Analista de Requerimientos se reúne con los stakeholders y usuarios finales.
2. Se documentan los requisitos funcionales y no funcionales.
3. Se revisa y aprueba el documento de requerimientos.

#### Caso de Uso 3: Diseño del Sistema

**Descripción:** Crear los modelos de datos, la arquitectura del sistema y los diseños de interfaz de usuario.

**Actores:** Arquitecto de Software, Diseñador de UI/UX.

**Flujo Principal:**
1. El Arquitecto de Software diseña la arquitectura del sistema.
2. El Diseñador de UI/UX crea los diseños de interfaz de usuario.
3. Se revisan y aprueban los diseños.

#### Caso de Uso 4: Implementación

**Descripción:** Desarrollar el código del sistema de carrito de compras según los diseños aprobados.

**Actores:** Desarrolladores.

**Flujo Principal:**
1. Los Desarrolladores escriben el código según los diseños y requerimientos.
2. Se realizan revisiones de código y pruebas unitarias.
3. Se integra el código en el repositorio principal.

#### Caso de Uso 5: Pruebas

**Descripción:** Realizar pruebas funcionales, de integración y de aceptación para asegurar la calidad del sistema.

**Actores:** Equipo de QA, Usuarios Finales.

**Flujo Principal:**
1. El Equipo de QA realiza pruebas funcionales y de integración.
2. Los Usuarios Finales realizan pruebas de aceptación.
3. Se documentan y corrigen los defectos encontrados.

#### Caso de Uso 6: Despliegue

**Descripción:** Implementar el sistema de carrito de compras en el entorno de producción.

**Actores:** Equipo de DevOps.

**Flujo Principal:**
1. El Equipo de DevOps prepara el entorno de producción.
2. Se despliega el sistema en producción.
3. Se monitorea el sistema para asegurar su correcto funcionamiento.

#### Caso de Uso 7: Mantenimiento

**Descripción:** Realizar mantenimiento y actualizaciones del sistema de carrito de compras.

**Actores:** Equipo de Soporte, Desarrolladores.

**Flujo Principal:**
1. El Equipo de Soporte monitorea el sistema y recibe reportes de incidencias.
2. Los Desarrolladores corrigen errores y realizan actualizaciones.
3. Se despliegan las actualizaciones en producción.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio del Proyecto] --> B[Planificación]
    B --> C[Análisis de Requerimientos]
    C --> D[Diseño del Sistema]
    D --> E[Implementación]
    E --> F[Pruebas]
    F --> G[Despliegue]
    G --> H[Mantenimiento]
    H --> I[Fin del Proyecto]
```

### Requisitos No Funcionales

1. **Escalabilidad:** El sistema debe ser capaz de manejar un aumento en la carga de usuarios y transacciones sin degradar el rendimiento.
2. **Seguridad:** El sistema debe proteger los datos de los usuarios y las transacciones contra accesos no autorizados y ataques.
3. **Rendimiento:** El sistema debe responder a las solicitudes de los usuarios en un tiempo razonable.
4. **Usabilidad:** La interfaz de usuario debe ser intuitiva y fácil de usar.
5. **Mantenibilidad:** El código del sistema debe ser modular y bien documentado para facilitar el mantenimiento y las actualizaciones.

## Implementación en Python

### Explicación Paso a Paso del Código

Para ilustrar cómo se puede implementar una parte del cronograma y las fases del proyecto en Python, vamos a desarrollar un script que ayude a gestionar las tareas y los hitos del proyecto. Este script permitirá crear, actualizar y visualizar tareas y hitos, y asignarles fechas de inicio y fin.

### Código Fuente Completo y Comentado

```python
import datetime

class Tarea:
    def __init__(self, nombre, descripcion, fecha_inicio, fecha_fin):
        self.nombre = nombre
        self.descripcion = descripcion
        self.fecha_inicio = fecha_inicio
        self.fecha_fin = fecha_fin
        self.completada = False

    def marcar_completada(self):
        self.completada = True

    def __str__(self):
        estado = "Completada" if self.completada else "Pendiente"
        return f"Tarea: {self.nombre}\nDescripción: {self.descripcion}\nInicio: {self.fecha_inicio}\nFin: {self.fecha_fin}\nEstado: {estado}"

class Proyecto:
    def __init__(self, nombre):
        self.nombre = nombre
        self.tareas = []

    def agregar_tarea(self, tarea):
        self.tareas.append(tarea)

    def listar_tareas(self):
        for tarea in self.tareas:
            print(tarea)

    def tareas_pendientes(self):
        return [tarea for tarea in self.tareas if not tarea.completada]

    def tareas_completadas(self):
        return [tarea for tarea in self.tareas if tarea.completada]

# Ejemplo de uso
if __name__ == "__main__":
    proyecto = Proyecto("Desarrollo del Sistema de Carrito de Compras")

    tarea1 = Tarea(
        nombre="Definir Alcance",
        descripcion="Reunión con stakeholders para definir el alcance del proyecto",
        fecha_inicio=datetime.date(2023, 10, 1),
        fecha_fin=datetime.date(2023, 10, 3)
    )

    tarea2 = Tarea(
        nombre="Análisis de Requerimientos",
        descripcion="Recopilar y documentar los requisitos del sistema",
        fecha_inicio=datetime.date(2023, 10, 4),
        fecha_fin=datetime.date(2023, 10, 10)
    )

    proyecto.agregar_tarea(tarea1)
    proyecto.agregar_tarea(tarea2)

    print("Tareas del Proyecto:")
    proyecto.listar_tareas()

    print("\nTareas Pendientes:")
    for tarea in proyecto.tareas_pendientes():
        print(tarea)

    tarea1.marcar_completada()

    print("\nTareas Completadas:")
    for tarea in proyecto.tareas_completadas():
        print(tarea)
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

El ejemplo de uso incluido en el código fuente muestra cómo crear un proyecto, agregar tareas y marcar tareas como completadas. Este ejemplo ayuda a visualizar cómo se puede gestionar el cronograma y las fases del proyecto de manera programática.

#### Pruebas Unitarias

Para asegurar la calidad del código, es importante escribir pruebas unitarias. A continuación, se presentan algunas pruebas unitarias básicas utilizando el módulo `unittest` de Python.

```python
import unittest

class TestProyecto(unittest.TestCase):
    def setUp(self):
        self.proyecto = Proyecto("Desarrollo del Sistema de Carrito de Compras")
        self.tarea1 = Tarea(
            nombre="Definir Alcance",
            descripcion="Reunión con stakeholders para definir el alcance del proyecto",
            fecha_inicio=datetime.date(2023, 10, 1),
            fecha_fin=datetime.date(2023, 10, 3)
        )
        self.tarea2 = Tarea(
            nombre="Análisis de Requerimientos",
            descripcion="Recopilar y documentar los requisitos del sistema",
            fecha_inicio=datetime.date(2023, 10, 4),
            fecha_fin=datetime.date(2023, 10, 10)
        )
        self.proyecto.agregar_tarea(self.tarea1)
        self.proyecto.agregar_tarea(self.tarea2)

    def test_agregar_tarea(self):
        self.assertEqual(len(self.proyecto.tareas), 2)

    def test_tareas_pendientes(self):
        self.assertEqual(len(self.proyecto.tareas_pendientes()), 2)
        self.tarea1.marcar_completada()
        self.assertEqual(len(self.proyecto.tareas_pendientes()), 1)

    def test_tareas_completadas(self):
        self.assertEqual(len(self.proyecto.tareas_completadas()), 0)
        self.tarea1.marcar_completada()
        self.assertEqual(len(self.proyecto.tareas_completadas()), 1)

if __name__ == "__main__":
    unittest.main()
```

## Mejores Prácticas y Consideraciones de Diseño

### Mejores Prácticas

1. **Planificación Detallada:** Dedicar tiempo suficiente a la planificación inicial del proyecto para evitar problemas durante la implementación.
2. **Comunicación Constante:** Mantener una comunicación constante y clara con todos los stakeholders y miembros del equipo.
3. **Gestión de Riesgos:** Identificar y gestionar los riesgos potenciales desde el inicio del proyecto.
4. **Revisiones Periódicas:** Realizar revisiones periódicas del progreso del proyecto y ajustar el cronograma según sea necesario.
5. **Documentación Completa:** Mantener una documentación completa y actualizada de todas las fases del proyecto.

### Consideraciones de Diseño

1. **Modularidad:** Diseñar el sistema de manera modular para facilitar el mantenimiento y las actualizaciones.
2. **Escalabilidad:** Asegurar que el diseño del sistema permita escalar fácilmente en respuesta a un aumento en la carga de usuarios y transacciones.
3. **Seguridad:** Implementar medidas de seguridad desde el diseño inicial para proteger los datos y las transacciones de los usuarios.
4. **Usabilidad:** Diseñar una interfaz de usuario intuitiva y fácil de usar para mejorar la experiencia del usuario final.
5. **Rendimiento:** Optimizar el rendimiento del sistema para asegurar tiempos de respuesta rápidos y eficientes.

### Consejos para la Depuración y Solución de Problemas Comunes

1. **Registro de Errores:** Implementar un sistema de registro de errores para facilitar la identificación y solución de problemas.
2. **Pruebas Unitarias:** Escribir pruebas unitarias para asegurar la calidad del código y detectar errores temprano.
3. **Revisiones de Código:** Realizar revisiones de código periódicas para identificar y corregir problemas de manera colaborativa.
4. **Monitoreo del Sistema:** Implementar herramientas de monitoreo para detectar problemas de rendimiento y disponibilidad en tiempo real.
5. **Documentación de Problemas:** Documentar los problemas encontrados y las soluciones implementadas para futuras referencias.

Con esta unidad, hemos cubierto en detalle el cronograma y las fases del proyecto para el desarrollo de un sistema de carrito de compras con Django. Al seguir estas directrices y mejores prácticas, los equipos de desarrollo pueden asegurar un proceso de desarrollo eficiente y exitoso.

# Unidad: Configuración del Proyecto Django

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en la configuración del proyecto Django, un aspecto crucial para el desarrollo de un sistema de carrito de compras. La configuración adecuada del proyecto no solo garantiza que la aplicación funcione correctamente, sino que también facilita su mantenimiento y escalabilidad. Al finalizar esta unidad, los estudiantes podrán:

1. Comprender la estructura de los archivos de configuración en un proyecto Django.
2. Configurar correctamente las aplicaciones dentro del proyecto.
3. Gestionar las configuraciones de desarrollo y producción.
4. Implementar configuraciones de seguridad y rendimiento.
5. Realizar pruebas y depuración de la configuración.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

La configuración del proyecto Django implica ajustar diversos parámetros en los archivos de configuración para asegurar que la aplicación funcione de manera óptima en diferentes entornos. Esto incluye la configuración de bases de datos, aplicaciones instaladas, middleware, rutas de URL, ajustes de seguridad y más.

### Casos de Uso

1. **Configuración Inicial del Proyecto**: El desarrollador configura el proyecto Django para que esté listo para el desarrollo.
2. **Ajuste de Configuraciones para Producción**: El desarrollador ajusta las configuraciones para que la aplicación sea segura y eficiente en un entorno de producción.
3. **Gestión de Aplicaciones**: El desarrollador añade, configura y gestiona las aplicaciones dentro del proyecto Django.
4. **Configuración de Seguridad**: El desarrollador implementa configuraciones de seguridad para proteger la aplicación contra amenazas comunes.
5. **Pruebas y Depuración**: El desarrollador realiza pruebas y depura la configuración para asegurar que todo funcione correctamente.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Crear Proyecto Django]
    B --> C[Configurar settings.py]
    C --> D[Configurar urls.py]
    D --> E[Configurar wsgi.py]
    E --> F[Configurar asgi.py]
    F --> G[Configurar aplicaciones]
    G --> H[Configurar base de datos]
    H --> I[Configurar seguridad]
    I --> J[Pruebas y Depuración]
    J --> K[Fin]
```

### Requisitos No Funcionales

1. **Rendimiento**: La configuración debe optimizar el rendimiento de la aplicación en producción.
2. **Seguridad**: La configuración debe incluir medidas de seguridad para proteger la aplicación.
3. **Escalabilidad**: La configuración debe permitir la escalabilidad del proyecto.
4. **Mantenibilidad**: La configuración debe ser clara y fácil de mantener.

## Implementación en Python

### Explicación Paso a Paso del Código

#### 1. Crear el Proyecto Django

Para crear un proyecto Django, utilizamos el comando `django-admin startproject nombre_del_proyecto`. Esto generará una estructura básica de directorios y archivos.

```bash
django-admin startproject carrito_compras
```

#### 2. Configurar `settings.py`

El archivo `settings.py` contiene todas las configuraciones del proyecto. Aquí configuramos la base de datos, aplicaciones instaladas, middleware, rutas estáticas, entre otros.

```python
# carrito_compras/settings.py

import os
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-4!$k&*#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!4&2@#5+5@3#4h@!

# Unidad: Configuración de `settings.py` en un Proyecto Django

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, profundizaremos en la configuración del archivo `settings.py` de un proyecto Django, específicamente en el contexto de un sistema de carrito de compras. El archivo `settings.py` es crucial para definir el comportamiento y las características del proyecto, incluyendo la configuración de la base de datos, las aplicaciones instaladas, la gestión de archivos estáticos y medios, y las configuraciones de seguridad.

### Objetivos de Aprendizaje

1. Comprender la estructura y propósito del archivo `settings.py`.
2. Configurar adecuadamente la base de datos para un sistema de carrito de compras.
3. Gestionar archivos estáticos y medios.
4. Configurar aplicaciones instaladas y middleware.
5. Implementar configuraciones de seguridad y rendimiento.
6. Realizar pruebas y depuración de la configuración.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El archivo `settings.py` de un proyecto Django contiene todas las configuraciones necesarias para que el proyecto funcione correctamente. En el contexto de un sistema de carrito de compras, es esencial configurar adecuadamente los siguientes aspectos:

- **Base de Datos**: Configuración de la base de datos para almacenar información de productos, usuarios, pedidos, etc.
- **Archivos Estáticos y Medios**: Gestión de archivos estáticos (CSS, JavaScript) y archivos de medios (imágenes de productos, documentos).
- **Aplicaciones Instaladas**: Lista de aplicaciones Django y de terceros necesarias para el funcionamiento del sistema.
- **Middleware**: Configuración de middleware para gestionar solicitudes y respuestas HTTP.
- **Seguridad**: Configuraciones de seguridad como claves secretas, HTTPS, y protección contra ataques comunes.
- **Internacionalización**: Configuración de idiomas y zonas horarias.
- **Otras Configuraciones**: Configuración de plantillas, autenticación, y cualquier otra configuración específica del proyecto.

### Casos de Uso

1. **Configuración Inicial del Proyecto**: Un desarrollador configura el archivo `settings.py` al iniciar un nuevo proyecto de carrito de compras.
2. **Actualización de Configuraciones**: Un desarrollador actualiza las configuraciones para agregar nuevas funcionalidades o mejorar el rendimiento.
3. **Gestión de Archivos Estáticos y Medios**: Un desarrollador configura la gestión de archivos estáticos y medios para asegurar que los recursos se carguen correctamente.
4. **Implementación de Seguridad**: Un desarrollador configura aspectos de seguridad para proteger el sistema contra ataques y vulnerabilidades.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Crear Proyecto Django]
    B --> C[Configurar settings.py]
    C --> D[Configurar Base de Datos]
    C --> E[Configurar Archivos Estáticos y Medios]
    C --> F[Configurar Aplicaciones Instaladas]
    C --> G[Configurar Middleware]
    C --> H[Configurar Seguridad]
    C --> I[Configurar Internacionalización]
    D --> J[Guardar Configuraciones]
    E --> J
    F --> J
    G --> J
    H --> J
    I --> J
    J --> K[Fin]
```

### Requisitos No Funcionales

1. **Rendimiento**: La configuración debe optimizar el rendimiento del sistema, especialmente en la gestión de archivos estáticos y medios.
2. **Seguridad**: La configuración debe seguir las mejores prácticas de seguridad para proteger el sistema y los datos de los usuarios.
3. **Escalabilidad**: La configuración debe permitir la escalabilidad del sistema para manejar un creciente número de usuarios y transacciones.
4. **Mantenibilidad**: La configuración debe ser clara y bien documentada para facilitar futuras actualizaciones y mantenimiento.

## Implementación en Python

### Explicación Paso a Paso del Código

A continuación, se presenta una explicación detallada de cómo configurar el archivo `settings.py` para un sistema de carrito de compras en Django.

#### 1. Configuración Básica

```python
import os
from pathlib import Path

# Ruta base del proyecto
BASE_DIR = Path(__file__).resolve().parent.parent

# Clave secreta para la seguridad del proyecto
SECRET_KEY = 'tu-clave-secreta-aqui'

# Modo de depuración
DEBUG = True

# Hosts permitidos
ALLOWED_HOSTS = []
```

- **BASE_DIR**: Define la ruta base del proyecto.
- **SECRET_KEY**: Clave secreta utilizada por Django para criptografía.
- **DEBUG**: Modo de depuración, debe ser `False` en producción.
- **ALLOWED_HOSTS**: Lista de hosts permitidos para el proyecto.

#### 2. Aplicaciones Instaladas

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'carrito',  # Aplicación del carrito de compras
    'productos',  # Aplicación de gestión de productos
    'usuarios',  # Aplicación de gestión de usuarios
    'pedidos',  # Aplicación de gestión de pedidos
    # Aplicaciones de terceros
    'rest_framework',
    'corsheaders',
]
```

- **INSTALLED_APPS**: Lista de aplicaciones Django y de terceros necesarias para el proyecto.

#### 3. Middleware

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'corsheaders.middleware.CorsMiddleware',
]
```

- **MIDDLEWARE**: Lista de middleware para gestionar solicitudes y respuestas HTTP.

#### 4. Configuración de la Base de Datos

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'carrito_db',
        'USER': 'tu_usuario',
        'PASSWORD': 'tu_contraseña',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

- **DATABASES**: Configuración de la base de datos, en este caso, PostgreSQL.

#### 5. Configuración de Archivos Estáticos y Medios

```python
# Archivos estáticos (CSS, JavaScript)
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "static"]

# Archivos de medios (imágenes, documentos)
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / "media"
```

- **STATIC_URL**: URL para acceder a archivos estáticos.
- **STATICFILES_DIRS**: Directorios donde se almacenan archivos estáticos.
- **MEDIA_URL**: URL para acceder a archivos de medios.
- **MEDIA_ROOT**: Directorio donde se almacenan archivos de medios.

#### 6. Configuración de Seguridad

```python
# Configuraciones de seguridad
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True
```

- **SECURE_BROWSER_XSS_FILTER**: Habilita el filtro XSS en el navegador.
- **X_FRAME_OPTIONS**: Protege contra ataques de clickjacking.
- **CSRF_COOKIE_SECURE**: Asegura la cookie CSRF.
- **SESSION_COOKIE_SECURE**: Asegura la cookie de sesión.

#### 7. Internacionalización

```python
# Configuración de idiomas y zonas horarias
LANGUAGE_CODE = 'es-ar'
TIME_ZONE = 'America/Argentina/Buenos_Aires'
USE_I18N = True
USE_L10N = True
USE_TZ = True
```

- **LANGUAGE_CODE**: Código de idioma.
- **TIME_ZONE**: Zona horaria.
- **USE_I18N**: Habilita la internacionalización.
- **USE_L10N**: Habilita la localización.
- **USE_TZ**: Habilita el uso de zonas horarias.

### Código Fuente Completo y Comentado

```python
import os
from pathlib import Path

# Ruta base del proyecto
BASE_DIR = Path(__file__).resolve().parent.parent

# Clave secreta para la seguridad del proyecto
SECRET_KEY = 'tu-clave-secreta-aqui'

# Modo de depuración
DEBUG = True

# Hosts permitidos
ALLOWED_HOSTS = []

# Aplicaciones instaladas
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'carrito',  # Aplicación del carrito de compras
    'productos',  # Aplicación de gestión de productos
    'usuarios',  # Aplicación de gestión de usuarios
    'pedidos',  # Aplicación de gestión de pedidos
    # Aplicaciones de terceros
    'rest_framework',
    'corsheaders',
]

# Middleware
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'corsheaders.middleware.CorsMiddleware',
]

# Configuración de la base de datos
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'carrito_db',
        'USER': 'tu_usuario',
        'PASSWORD': 'tu_contraseña',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}

# Archivos estáticos (CSS, JavaScript)
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "static"]

# Archivos de medios (imágenes, documentos)
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / "media"

# Configuraciones de seguridad
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True

# Configuración de idiomas y zonas horarias
LANGUAGE_CODE = 'es-ar'
TIME_ZONE = 'America/Argentina/Buenos_Aires'
USE_I18N = True
USE_L10N = True
USE_TZ = True
```

### Ejemplos de Uso y Pruebas Unitarias

Para probar la configuración del archivo `settings.py`, se pueden realizar las siguientes acciones:

1. **Verificar la Conexión a la Base de Datos**: Ejecutar `python manage.py migrate` para asegurarse de que la configuración de la base de datos es correcta.
2. **Probar la Carga de Archivos Estáticos**: Crear un archivo CSS en el directorio `static` y verificar que se carga correctamente en una plantilla.
3. **Subir y Acceder a Archivos de Medios**: Subir una imagen a través de la interfaz de administración y verificar que se almacena en el directorio `media` y se puede acceder a través de la URL configurada.
4. **Probar Configuraciones de Seguridad**: Verificar que las configuraciones de seguridad están activas utilizando herramientas de análisis de seguridad.

#### Pruebas Unitarias

```python
from django.test import TestCase
from django.conf import settings

class SettingsTestCase(TestCase):
    def test_debug_is_false_in_production(self):
        self.assertFalse(settings.DEBUG, "DEBUG debe ser False en producción")

    def test_database_configuration(self):
        self.assertEqual(settings.DATABASES['default']['ENGINE'], 'django.db.backends.postgresql')
        self.assertEqual(settings.DATABASES['default']['NAME'], 'carrito_db')

    def test_static_files_configuration(self):
        self.assertEqual(settings.STATIC_URL, '/static/')
        self.assertIn(settings.BASE_DIR / "static", settings.STATICFILES_DIRS)

    def test_media_files_configuration(self):
        self.assertEqual(settings.MEDIA_URL, '/media/')
        self.assertEqual(settings.MEDIA_ROOT, settings.BASE_DIR / "media")

    def test_security_configuration(self):
        self.assertTrue(settings.SECURE_BROWSER_XSS_FILTER)
        self.assertEqual(settings.X_FRAME_OPTIONS, 'DENY')
        self.assertTrue(settings.CSRF_COOKIE_SECURE)
        self.assertTrue(settings.SESSION_COOKIE_SECURE)

    def test_internationalization_configuration(self):
        self.assertEqual(settings.LANGUAGE_CODE, 'es-ar')
        self.assertEqual(settings.TIME_ZONE, 'America/Argentina/Buenos_Aires')
        self.assertTrue(settings.USE_I18N)
        self.assertTrue(settings.USE_L10N)
        self.assertTrue(settings.USE_TZ)
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Separación de Configuraciones**: Utilizar diferentes archivos de configuración para desarrollo y producción. Por ejemplo, `settings_dev.py` y `settings_prod.py`.
2. **Variables de Entorno**: Utilizar variables de entorno para almacenar información sensible como claves secretas y credenciales de la base de datos.
3. **Gestión de Archivos Estáticos y Medios**: Utilizar servicios de almacenamiento en la nube para archivos estáticos y medios en producción.
4. **Seguridad**: Mantener siempre `DEBUG` en `False` en producción y configurar adecuadamente los hosts permitidos.
5. **Documentación**: Documentar todas las configuraciones en el archivo `settings.py` para facilitar el mantenimiento y la colaboración en equipo.
6. **Pruebas**: Realizar pruebas unitarias para verificar que las configuraciones son correctas y funcionan como se espera.

### Consideraciones de Seguridad

- **Claves Secretas**: Nunca exponer la clave secreta en el código fuente. Utilizar variables de entorno para almacenarla.
- **HTTPS**: Configurar el proyecto para utilizar HTTPS en producción.
- **Protección CSRF**: Asegurarse de que la protección CSRF está habilitada y configurada correctamente.
- **Control de Acceso**: Configurar adecuadamente los permisos y autenticación para proteger los datos de los usuarios.

### Consideraciones de Rendimiento

- **Caché**: Utilizar caché para mejorar el rendimiento del sistema.
- **Compresión de Archivos**: Comprimir archivos estáticos para reducir el tiempo de carga.
- **Optimización de Consultas**: Optimizar las consultas a la base de datos para mejorar el rendimiento.

### Escalabilidad

- **Base de Datos**: Utilizar una base de datos escalable como PostgreSQL.
- **Servicios en la Nube**: Considerar el uso de servicios en la nube para almacenamiento y procesamiento.
- **Balanceo de Carga**: Implementar balanceo de carga para distribuir el tráfico entre múltiples servidores.

### Depuración y Solución de Problemas Comunes

- **Errores de Conexión a la Base de Datos**: Verificar las credenciales y la configuración de la base de datos.
- **Problemas con Archivos Estáticos**: Asegurarse de que los archivos están en el directorio correcto y que las URLs están configuradas adecuadamente.
- **Problemas de Seguridad**: Utilizar herramientas de análisis de seguridad para identificar y solucionar vulnerabilidades.

Con esta unidad, se espera que los desarrolladores tengan una comprensión profunda de cómo configurar el archivo `settings.py` en un proyecto Django, específicamente en el contexto de un sistema de carrito de compras. La correcta configuración de este archivo es esencial para el funcionamiento, seguridad y rendimiento del sistema.

# Unidad: Gestión de dependencias con requirements.txt

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, exploraremos en profundidad la gestión de dependencias en un proyecto Django utilizando el archivo `requirements.txt`. La gestión adecuada de dependencias es crucial para garantizar que nuestro proyecto sea reproducible y fácil de mantener. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender la importancia de la gestión de dependencias en proyectos Django.
2. Crear y mantener un archivo `requirements.txt`.
3. Instalar dependencias utilizando `pip`.
4. Actualizar y eliminar dependencias de manera segura.
5. Implementar mejores prácticas para la gestión de dependencias.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

La gestión de dependencias en un proyecto Django se realiza mediante un archivo denominado `requirements.txt`. Este archivo contiene una lista de todas las librerías y sus versiones específicas que el proyecto necesita para funcionar correctamente. La correcta gestión de este archivo asegura que cualquier desarrollador que trabaje en el proyecto pueda instalar todas las dependencias necesarias de manera rápida y eficiente.

### Casos de uso

1. **Instalación inicial de dependencias**: Un desarrollador clona el repositorio del proyecto y necesita instalar todas las dependencias para comenzar a trabajar.
2. **Actualización de dependencias**: Se requiere actualizar una o varias librerías a versiones más recientes sin romper la funcionalidad existente.
3. **Eliminación de dependencias**: Se decide que una librería ya no es necesaria y debe ser eliminada del proyecto.
4. **Reproducción de entorno**: Otro desarrollador o un servidor de producción necesita replicar el entorno de desarrollo para ejecutar el proyecto.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Crear archivo requirements.txt]
    B --> C[Agregar dependencias al archivo]
    C --> D[Instalar dependencias con pip]
    D --> E[Actualizar dependencias]
    E --> F[Eliminar dependencias]
    F --> G[Revisar y probar el proyecto]
    G --> H[Fin]
```

### Requisitos no funcionales

1. **Reproducibilidad**: El entorno de desarrollo debe ser replicable en cualquier máquina.
2. **Compatibilidad**: Las versiones de las librerías deben ser compatibles entre sí y con la versión de Django utilizada.
3. **Seguridad**: Las dependencias deben ser seguras y libres de vulnerabilidades conocidas.
4. **Mantenibilidad**: El archivo `requirements.txt` debe ser fácil de leer y mantener.

## Implementación en Python

### Explicación paso a paso del código

#### Paso 1: Crear el archivo `requirements.txt`

El archivo `requirements.txt` se puede crear manualmente o utilizando comandos de `pip`. Para crear el archivo manualmente, simplemente crea un archivo de texto llamado `requirements.txt` en el directorio raíz de tu proyecto Django.

#### Paso 2: Agregar dependencias al archivo

Puedes agregar dependencias al archivo `requirements.txt` especificando el nombre de la librería y opcionalmente la versión que deseas utilizar. Por ejemplo:

```
Django==3.2.5
djangorestframework==3.12.4
```

#### Paso 3: Instalar dependencias utilizando `pip`

Para instalar todas las dependencias listadas en el archivo `requirements.txt`, utiliza el siguiente comando:

```bash
pip install -r requirements.txt
```

#### Paso 4: Actualizar dependencias

Para actualizar una dependencia específica, puedes modificar la versión en el archivo `requirements.txt` y luego ejecutar nuevamente el comando de instalación. También puedes utilizar `pip` para actualizar directamente:

```bash
pip install --upgrade nombre_libreria
```

#### Paso 5: Eliminar dependencias

Para eliminar una dependencia, simplemente remuévela del archivo `requirements.txt` y ejecuta el siguiente comando para desinstalarla:

```bash
pip uninstall nombre_libreria
```

### Código fuente completo y comentado

A continuación, se presenta un ejemplo completo de un archivo `requirements.txt` y los comandos necesarios para su gestión.

#### Archivo `requirements.txt`

```plaintext
# Archivo de dependencias para el proyecto Django
Django==3.2.5
djangorestframework==3.12.4
psycopg2-binary==2.8.6
```

#### Comandos de gestión

```bash
# Instalar todas las dependencias
pip install -r requirements.txt

# Actualizar una dependencia específica
pip install --upgrade djangorestframework

# Eliminar una dependencia
pip uninstall psycopg2-binary
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Supongamos que un nuevo desarrollador se une al proyecto y necesita configurar su entorno de desarrollo. Los pasos serían los siguientes:

1. Clonar el repositorio del proyecto:

```bash
git clone https://github.com/usuario/proyecto-django.git
cd proyecto-django
```

2. Crear un entorno virtual (recomendado):

```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

3. Instalar las dependencias:

```bash
pip install -r requirements.txt
```

#### Pruebas unitarias

Para asegurar que las dependencias se instalan correctamente, podemos escribir un script de prueba que verifique la instalación de las librerías necesarias.

```python
import django
import rest_framework
import psycopg2

def test_dependencies():
    assert django.get_version() == '3.2.5'
    assert rest_framework.VERSION == '3.12.4'
    assert psycopg2.__version__ == '2.8.6'

if __name__ == "__main__":
    test_dependencies()
    print("Todas las dependencias están correctamente instaladas.")
```

## Mejores prácticas y consideraciones de diseño

1. **Utilizar entornos virtuales**: Siempre utiliza entornos virtuales para aislar las dependencias del proyecto y evitar conflictos con otras librerías instaladas globalmente.
2. **Fijar versiones específicas**: Fija versiones específicas de las librerías en el archivo `requirements.txt` para asegurar la reproducibilidad del entorno.
3. **Revisar regularmente las dependencias**: Mantén las dependencias actualizadas y revisa regularmente las vulnerabilidades de seguridad.
4. **Documentar las dependencias**: Documenta cualquier cambio en las dependencias y el motivo detrás de la actualización o eliminación de una librería.
5. **Automatizar la instalación**: Utiliza scripts o herramientas de automatización para simplificar la instalación y actualización de dependencias.

### Consideraciones sobre seguridad, rendimiento y escalabilidad

- **Seguridad**: Revisa regularmente las dependencias en busca de vulnerabilidades conocidas utilizando herramientas como `pip-audit` o servicios de análisis de dependencias.
- **Rendimiento**: Algunas librerías pueden tener un impacto significativo en el rendimiento del proyecto. Realiza pruebas de rendimiento después de agregar o actualizar dependencias críticas.
- **Escalabilidad**: Asegúrate de que las dependencias sean escalables y puedan manejar el crecimiento del proyecto sin problemas.

### Consejos para la depuración y solución de problemas comunes

- **Errores de instalación**: Si encuentras errores al instalar dependencias, verifica que el archivo `requirements.txt` no contenga errores tipográficos y que las versiones especificadas sean correctas.
- **Conflictos de versiones**: Si hay conflictos de versiones entre librerías, considera utilizar un archivo `constraints.txt` para fijar versiones compatibles.
- **Entornos virtuales**: Si las dependencias no se instalan correctamente, asegúrate de que el entorno virtual esté activado y configurado correctamente.

Con estos conocimientos y prácticas, estarás bien equipado para gestionar las dependencias de tu proyecto Django de manera eficiente y segura.

# Unidad: Configuración de base de datos (PostgreSQL recomendado)

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la configuración de la base de datos para nuestro proyecto Django, específicamente utilizando PostgreSQL. PostgreSQL es una de las bases de datos relacionales más robustas y populares en la industria, conocida por su rendimiento, escalabilidad y características avanzadas. Al finalizar esta unidad, los lectores estarán capacitados para:

1. Configurar PostgreSQL en su entorno de desarrollo.
2. Integrar PostgreSQL con un proyecto Django.
3. Realizar operaciones básicas de administración de bases de datos.
4. Comprender las mejores prácticas y consideraciones de diseño para el uso de PostgreSQL con Django.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es configurar una base de datos PostgreSQL para un proyecto Django. Esto incluye la instalación de PostgreSQL, la creación de una base de datos y un usuario, y la modificación de la configuración del proyecto Django para utilizar esta base de datos. Además, se abordarán aspectos de seguridad, rendimiento y escalabilidad.

### b. Casos de uso

1. **Caso de uso 1: Instalación de PostgreSQL**
   - **Actor:** Desarrollador
   - **Descripción:** El desarrollador instala PostgreSQL en su entorno de desarrollo.
   - **Precondiciones:** El desarrollador tiene acceso a su entorno de desarrollo.
   - **Postcondiciones:** PostgreSQL está instalado y en funcionamiento.

2. **Caso de uso 2: Creación de base de datos y usuario**
   - **Actor:** Desarrollador
   - **Descripción:** El desarrollador crea una nueva base de datos y un usuario en PostgreSQL.
   - **Precondiciones:** PostgreSQL está instalado y en funcionamiento.
   - **Postcondiciones:** La base de datos y el usuario están creados y configurados.

3. **Caso de uso 3: Configuración de Django para usar PostgreSQL**
   - **Actor:** Desarrollador
   - **Descripción:** El desarrollador configura el proyecto Django para utilizar la base de datos PostgreSQL.
   - **Precondiciones:** La base de datos y el usuario están creados.
   - **Postcondiciones:** Django está configurado para utilizar PostgreSQL como su base de datos.

4. **Caso de uso 4: Pruebas de conexión y migraciones**
   - **Actor:** Desarrollador
   - **Descripción:** El desarrollador prueba la conexión a la base de datos y realiza migraciones.
   - **Precondiciones:** Django está configurado para usar PostgreSQL.
   - **Postcondiciones:** La conexión a la base de datos es exitosa y las migraciones se realizan correctamente.

### c. Diagramas de flujo (en mermaid)

```mermaid
flowchart TD
    A[Inicio] --> B[Instalación de PostgreSQL]
    B --> C[Creación de base de datos y usuario]
    C --> D[Configuración de Django para usar PostgreSQL]
    D --> E[Pruebas de conexión y migraciones]
    E --> F[Fin]
```

### d. Requisitos no funcionales

1. **Seguridad:**
   - La configuración debe asegurar que las credenciales de la base de datos no se expongan en el código fuente.
   - Se deben seguir las mejores prácticas para la gestión de usuarios y permisos en PostgreSQL.

2. **Rendimiento:**
   - La configuración debe optimizar el rendimiento de las consultas a la base de datos.
   - Se deben considerar índices y particionamiento si es necesario.

3. **Escalabilidad:**
   - La configuración debe permitir la escalabilidad horizontal y vertical de la base de datos.
   - Se debe considerar el uso de réplicas y balanceo de carga.

4. **Mantenibilidad:**
   - La configuración debe ser clara y bien documentada para facilitar el mantenimiento y la actualización.

## Implementación en Python

### a. Explicación paso a paso del código

Para configurar PostgreSQL en un proyecto Django, seguiremos los siguientes pasos:

1. **Instalación de PostgreSQL:**
   - En sistemas basados en Debian/Ubuntu:
     ```bash
     sudo apt update
     sudo apt install postgresql postgresql-contrib
     ```
   - En sistemas basados en Red Hat/Fedora:
     ```bash
     sudo dnf install postgresql-server postgresql-contrib
     sudo postgresql-setup --initdb
     sudo systemctl start postgresql
     sudo systemctl enable postgresql
     ```

2. **Creación de base de datos y usuario:**
   - Acceder a la consola de PostgreSQL:
     ```bash
     sudo -i -u postgres
     psql
     ```
   - Crear una base de datos y un usuario:
     ```sql
     CREATE DATABASE carrito_compras;
     CREATE USER carrito_user WITH PASSWORD 'password123';
     ALTER ROLE carrito_user SET client_encoding TO 'utf8';
     ALTER ROLE carrito_user SET default_transaction_isolation TO 'read committed';
     ALTER ROLE carrito_user SET timezone TO 'UTC';
     GRANT ALL PRIVILEGES ON DATABASE carrito_compras TO carrito_user;
     ```

3. **Configuración de Django para usar PostgreSQL:**
   - Instalar el adaptador de PostgreSQL para Django:
     ```bash
     pip install psycopg2-binary
     ```
   - Modificar el archivo `settings.py` del proyecto Django:
     ```python
     DATABASES = {
         'default': {
             'ENGINE': 'django.db.backends.postgresql',
             'NAME': 'carrito_compras',
             'USER': 'carrito_user',
             'PASSWORD': 'password123',
             'HOST': 'localhost',
             'PORT': '5432',
         }
     }
     ```

4. **Pruebas de conexión y migraciones:**
   - Ejecutar las migraciones de Django:
     ```bash
     python manage.py makemigrations
     python manage.py migrate
     ```

### b. Código fuente completo y comentado

```python
# settings.py

# Configuración de la base de datos PostgreSQL
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',  # Motor de base de datos
        'NAME': 'carrito_compras',  # Nombre de la base de datos
        'USER': 'carrito_user',  # Usuario de la base de datos
        'PASSWORD': 'password123',  # Contraseña del usuario
        'HOST': 'localhost',  # Dirección del servidor de la base de datos
        'PORT': '5432',  # Puerto del servidor de la base de datos
    }
}

# Otros ajustes de configuración...
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Una vez configurada la base de datos, podemos crear un modelo en Django y verificar que las operaciones de base de datos funcionan correctamente.

```python
# models.py

from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    descripcion = models.TextField()

    def __str__(self):
        return self.nombre
```

#### Pruebas unitarias

Podemos escribir pruebas unitarias para verificar que la conexión a la base de datos y las operaciones CRUD funcionan correctamente.

```python
# tests.py

from django.test import TestCase
from .models import Producto

class ProductoModelTest(TestCase):

    def setUp(self):
        Producto.objects.create(nombre="Producto 1", precio=100.00, descripcion="Descripción del producto 1")

    def test_producto_creacion(self):
        producto = Producto.objects.get(nombre="Producto 1")
        self.assertEqual(producto.nombre, "Producto 1")
        self.assertEqual(producto.precio, 100.00)
        self.assertEqual(producto.descripcion, "Descripción del producto 1")
```

### d. Mejores prácticas y consideraciones de diseño

1. **Gestión de credenciales:**
   - Utilizar variables de entorno para almacenar las credenciales de la base de datos en lugar de incluirlas directamente en el archivo `settings.py`. Esto mejora la seguridad y facilita la gestión de diferentes entornos (desarrollo, pruebas, producción).

     ```python
     import os

     DATABASES = {
         'default': {
             'ENGINE': 'django.db.backends.postgresql',
             'NAME': os.getenv('DB_NAME', 'carrito_compras'),
             'USER': os.getenv('DB_USER', 'carrito_user'),
             'PASSWORD': os.getenv('DB_PASSWORD', 'password123'),
             'HOST': os.getenv('DB_HOST', 'localhost'),
             'PORT': os.getenv('DB_PORT', '5432'),
         }
     }
     ```

2. **Optimización de consultas:**
   - Utilizar el ORM de Django para realizar consultas eficientes y aprovechar las características avanzadas de PostgreSQL, como índices y particionamiento.
   - Analizar y optimizar las consultas SQL generadas por el ORM utilizando herramientas como `EXPLAIN`.

3. **Escalabilidad:**
   - Considerar el uso de réplicas de lectura para distribuir la carga de trabajo.
   - Implementar particionamiento de tablas para manejar grandes volúmenes de datos.
   - Utilizar herramientas de monitoreo y ajuste de rendimiento, como pgAdmin y pgtune.

4. **Seguridad:**
   - Configurar roles y permisos adecuados en PostgreSQL para limitar el acceso a la base de datos.
   - Utilizar conexiones seguras (SSL/TLS) para proteger la comunicación entre Django y PostgreSQL.
   - Realizar copias de seguridad periódicas de la base de datos y probar los procedimientos de recuperación.

5. **Mantenibilidad:**
   - Documentar la configuración y los procedimientos de administración de la base de datos.
   - Mantener el software actualizado y aplicar parches de seguridad de manera oportuna.
   - Automatizar tareas comunes de administración, como la creación de copias de seguridad y la actualización de índices.

## Conclusión

En esta unidad, hemos cubierto todos los aspectos necesarios para configurar una base de datos PostgreSQL en un proyecto Django. Desde la instalación y configuración inicial hasta la integración con Django y las mejores prácticas para la seguridad, el rendimiento y la escalabilidad. Siguiendo estos pasos y consideraciones, los desarrolladores pueden asegurarse de que su proyecto Django esté bien preparado para manejar datos de manera eficiente y segura.

# Unidad: Configuración de entornos de desarrollo, pruebas y producción

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la configuración de entornos de desarrollo, pruebas y producción para un proyecto Django, específicamente para un sistema de carrito de compras. La correcta configuración de estos entornos es crucial para garantizar que el desarrollo sea eficiente, las pruebas sean precisas y la producción sea segura y escalable.

### Objetivos de aprendizaje:

1. Comprender la importancia de los diferentes entornos en un proyecto Django.
2. Configurar adecuadamente los entornos de desarrollo, pruebas y producción.
3. Implementar buenas prácticas para la gestión de configuraciones específicas de cada entorno.
4. Desarrollar un flujo de trabajo que facilite la transición entre los diferentes entornos.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

La configuración de entornos de desarrollo, pruebas y producción en Django implica la creación de configuraciones específicas para cada entorno, asegurando que cada uno tenga los ajustes necesarios para su propósito. Esto incluye la configuración de bases de datos, variables de entorno, ajustes de seguridad, y otros parámetros que varían según el entorno.

### Casos de uso

1. **Desarrollo**: Los desarrolladores necesitan un entorno donde puedan escribir y probar código de manera rápida y eficiente. Este entorno debe ser flexible y permitir la depuración fácil.
2. **Pruebas**: Los testers necesitan un entorno que simule la producción lo más fielmente posible para realizar pruebas unitarias, de integración y funcionales.
3. **Producción**: El entorno de producción debe ser seguro, eficiente y escalable, con configuraciones optimizadas para el rendimiento y la seguridad.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Configurar entorno de desarrollo]
    B --> C[Configurar entorno de pruebas]
    C --> D[Configurar entorno de producción]
    D --> E[Verificar configuraciones]
    E --> F[Despliegue en producción]
    F --> G[Monitoreo y mantenimiento]
```

### Requisitos no funcionales

1. **Seguridad**: Las configuraciones de producción deben incluir medidas de seguridad robustas, como el uso de HTTPS, protección contra ataques CSRF y XSS, y manejo seguro de las credenciales.
2. **Rendimiento**: El entorno de producción debe estar optimizado para manejar grandes volúmenes de tráfico y minimizar el tiempo de respuesta.
3. **Escalabilidad**: La arquitectura debe permitir la fácil escalabilidad horizontal y vertical para adaptarse a un aumento en la carga.
4. **Mantenibilidad**: Las configuraciones deben ser claras y bien documentadas para facilitar el mantenimiento y las actualizaciones.

## Implementación en Python

### Explicación paso a paso del código

Para gestionar las configuraciones de los diferentes entornos, utilizaremos el patrón de configuración basada en entornos. Esto implica tener un archivo de configuración base (`base.py`) y archivos específicos para cada entorno (`development.py`, `testing.py`, `production.py`).

1. **Crear el directorio de configuración**:
   En el directorio del proyecto, crea un directorio llamado `config` donde se almacenarán los archivos de configuración.

2. **Archivo base de configuración (`base.py`)**:
   Este archivo contendrá las configuraciones comunes a todos los entornos.

3. **Archivos específicos de cada entorno**:
   Crea archivos `development.py`, `testing.py` y `production.py` que extiendan la configuración base y añadan o modifiquen configuraciones específicas para cada entorno.

### Código fuente completo y comentado

#### `config/base.py`

```python
# config/base.py

import os
from pathlib import Path

# Ruta base del proyecto
BASE_DIR = Path(__file__).resolve().parent.parent

# Configuración común a todos los entornos
SECRET_KEY = os.getenv('DJANGO_SECRET_KEY', 'default-secret-key')
DEBUG = False
ALLOWED_HOSTS = []

# Aplicaciones instaladas
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'shop',  # Aplicación del carrito de compras
]

# Middleware
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# URL de configuración
ROOT_URLCONF = 'myproject.urls'

# Plantillas
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# WSGI
WSGI_APPLICATION = 'myproject.wsgi.application'

# Base de datos
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('DB_NAME', 'mydatabase'),
        'USER': os.getenv('DB_USER', 'mydatabaseuser'),
        'PASSWORD': os.getenv('DB_PASSWORD', 'mypassword'),
        'HOST': os.getenv('DB_HOST', 'localhost'),
        'PORT': os.getenv('DB_PORT', '5432'),
    }
}

# Validadores de contraseñas
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# Internacionalización
LANGUAGE_CODE = 'es-ar'
TIME_ZONE = 'America/Argentina/Buenos_Aires'
USE_I18N = True
USE_L10N = True
USE_TZ = True

# Archivos estáticos
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']

# Archivos multimedia
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

#### `config/development.py`

```python
# config/development.py

from .base import *

# Configuración específica para el entorno de desarrollo
DEBUG = True
ALLOWED_HOSTS = ['localhost', '127.0.0.1']

# Configuración de la base de datos para desarrollo
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'dev_mydatabase',
        'USER': 'dev_mydatabaseuser',
        'PASSWORD': 'dev_mypassword',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}

# Configuración de correo para desarrollo
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
```

#### `config/testing.py`

```python
# config/testing.py

from .base import *

# Configuración específica para el entorno de pruebas
DEBUG = False
ALLOWED_HOSTS = ['testserver']

# Configuración de la base de datos para pruebas
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'test_mydatabase',
        'USER': 'test_mydatabaseuser',
        'PASSWORD': 'test_mypassword',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}

# Configuración de correo para pruebas
EMAIL_BACKEND = 'django.core.mail.backends.locmem.EmailBackend'
```

#### `config/production.py`

```python
# config/production.py

from .base import *

# Configuración específica para el entorno de producción
DEBUG = False
ALLOWED_HOSTS = ['myproductiondomain.com']

# Configuración de la base de datos para producción
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('DB_NAME'),
        'USER': os.getenv('DB_USER'),
        'PASSWORD': os.getenv('DB_PASSWORD'),
        'HOST': os.getenv('DB_HOST'),
        'PORT': os.getenv('DB_PORT'),
    }
}

# Configuración de correo para producción
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = os.getenv('EMAIL_HOST')
EMAIL_PORT = os.getenv('EMAIL_PORT')
EMAIL_USE_TLS = os.getenv('EMAIL_USE_TLS', True)
EMAIL_HOST_USER = os.getenv('EMAIL_HOST_USER')
EMAIL_HOST_PASSWORD = os.getenv('EMAIL_HOST_PASSWORD')

# Seguridad adicional para producción
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

### Ejemplos de uso y pruebas unitarias

Para cambiar entre los diferentes entornos, se puede utilizar la variable de entorno `DJANGO_SETTINGS_MODULE` al ejecutar los comandos de Django.

#### Ejemplo de uso en desarrollo

```bash
export DJANGO_SETTINGS_MODULE=config.development
python manage.py runserver
```

#### Ejemplo de uso en pruebas

```bash
export DJANGO_SETTINGS_MODULE=config.testing
python manage.py test
```

#### Ejemplo de uso en producción

```bash
export DJANGO_SETTINGS_MODULE=config.production
gunicorn myproject.wsgi
```

### Pruebas unitarias

Para asegurar que las configuraciones funcionan correctamente, se deben realizar pruebas unitarias. A continuación, se muestra un ejemplo de cómo se pueden realizar pruebas básicas para verificar las configuraciones.

```python
# tests/test_settings.py

from django.conf import settings
from django.test import TestCase

class SettingsTestCase(TestCase):
    def test_development_settings(self):
        self.assertTrue(settings.DEBUG)
        self.assertIn('localhost', settings.ALLOWED_HOSTS)

    def test_testing_settings(self):
        self.assertFalse(settings.DEBUG)
        self.assertIn('testserver', settings.ALLOWED_HOSTS)

    def test_production_settings(self):
        self.assertFalse(settings.DEBUG)
        self.assertIn('myproductiondomain.com', settings.ALLOWED_HOSTS)
        self.assertTrue(settings.SECURE_SSL_REDIRECT)
```

## Mejores prácticas y consideraciones de diseño

### Separación de configuraciones

Es fundamental mantener las configuraciones separadas para cada entorno. Esto no solo facilita la gestión y el mantenimiento, sino que también reduce el riesgo de errores y problemas de seguridad.

### Uso de variables de entorno

Las variables de entorno son una forma segura y flexible de manejar configuraciones sensibles, como credenciales de bases de datos y claves secretas. Utiliza librerías como `python-decouple` o `django-environ` para manejar las variables de entorno de manera eficiente.

### Seguridad en producción

Asegúrate de que las configuraciones de producción incluyan todas las medidas de seguridad necesarias, como el uso de HTTPS, la configuración de cookies seguras y la protección contra ataques comunes.

### Monitoreo y mantenimiento

Implementa herramientas de monitoreo y logging para el entorno de producción. Esto te permitirá detectar y solucionar problemas rápidamente, asegurando la estabilidad y el rendimiento del sistema.

### Automatización del despliegue

Utiliza herramientas de automatización como Docker, Ansible o Terraform para gestionar el despliegue de tu aplicación en producción. Esto no solo facilita el proceso, sino que también reduce el riesgo de errores humanos.

### Depuración y solución de problemas comunes

1. **Errores de configuración**: Verifica que todas las variables de entorno estén correctamente configuradas y que los archivos de configuración sean válidos.
2. **Problemas de conexión a la base de datos**: Asegúrate de que las credenciales y la configuración de la base de datos sean correctas y que el servidor de la base de datos esté accesible.
3. **Problemas de rendimiento**: Utiliza herramientas de profiling y monitoreo para identificar cuellos de botella y optimizar el rendimiento de la aplicación.

Con estas prácticas y consideraciones, estarás bien preparado para configurar y gestionar los entornos de desarrollo, pruebas y producción de tu proyecto Django de manera eficiente y segura.

# Unidad: Modelo de Usuario (extensión de AbstractUser) - Sistema de Carrito de Compras con Django

## 1. Introducción a la unidad y objetivos de aprendizaje

En esta unidad, profundizaremos en la creación y personalización de un modelo de usuario en Django mediante la extensión de `AbstractUser`. Este modelo será fundamental para la implementación de un sistema de carrito de compras, ya que permitirá gestionar la autenticación y autorización de los usuarios, así como almacenar información adicional relevante para el sistema.

### Objetivos de aprendizaje

1. Comprender la importancia de personalizar el modelo de usuario en Django.
2. Aprender a extender el modelo `AbstractUser` para añadir campos personalizados.
3. Implementar un modelo de usuario personalizado en un proyecto Django.
4. Configurar adecuadamente el proyecto Django para utilizar el modelo de usuario personalizado.
5. Realizar pruebas unitarias para asegurar el correcto funcionamiento del modelo de usuario.

## 2. Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

El modelo de usuario personalizado permitirá gestionar la autenticación y autorización de los usuarios en el sistema de carrito de compras. Además, se añadirán campos adicionales para almacenar información relevante, como la dirección de envío y el número de teléfono. Este modelo será utilizado en todo el sistema para identificar y autenticar a los usuarios, así como para almacenar información personalizada.

### b. Casos de uso

#### Caso de uso 1: Registro de usuario

**Descripción:** Un nuevo usuario se registra en el sistema proporcionando su información personal y de contacto.

**Actores:** Usuario

**Precondiciones:**
- El usuario no debe estar registrado previamente.

**Flujo principal:**
1. El usuario accede a la página de registro.
2. El usuario completa el formulario de registro con su nombre de usuario, contraseña, dirección de correo electrónico, dirección de envío y número de teléfono.
3. El sistema valida la información proporcionada.
4. El sistema crea una nueva cuenta de usuario y almacena la información en la base de datos.
5. El sistema redirige al usuario a la página de inicio de sesión.

**Postcondiciones:**
- El usuario está registrado en el sistema y puede iniciar sesión.

#### Caso de uso 2: Inicio de sesión de usuario

**Descripción:** Un usuario registrado inicia sesión en el sistema proporcionando su nombre de usuario y contraseña.

**Actores:** Usuario

**Precondiciones:**
- El usuario debe estar registrado en el sistema.

**Flujo principal:**
1. El usuario accede a la página de inicio de sesión.
2. El usuario ingresa su nombre de usuario y contraseña.
3. El sistema valida las credenciales proporcionadas.
4. El sistema autentica al usuario y lo redirige a la página principal del sistema.

**Postcondiciones:**
- El usuario está autenticado y puede acceder a las funcionalidades del sistema.

### c. Diagramas de flujo (en mermaid)

```mermaid
flowchart TD
    A[Usuario accede a la página de registro] --> B[Usuario completa el formulario de registro]
    B --> C[El sistema valida la información]
    C --> D{Información válida?}
    D -->|Sí| E[El sistema crea una nueva cuenta de usuario]
    E --> F[El sistema redirige al usuario a la página de inicio de sesión]
    D -->|No| G[El sistema muestra un mensaje de error]

    H[Usuario accede a la página de inicio de sesión] --> I[Usuario ingresa nombre de usuario y contraseña]
    I --> J[El sistema valida las credenciales]
    J --> K{Credenciales válidas?}
    K -->|Sí| L[El sistema autentica al usuario]
    L --> M[El sistema redirige al usuario a la página principal]
    K -->|No| N[El sistema muestra un mensaje de error]
```

### d. Requisitos no funcionales

1. **Seguridad:** El sistema debe asegurar que las contraseñas de los usuarios estén encriptadas y almacenadas de manera segura.
2. **Rendimiento:** El sistema debe ser capaz de manejar múltiples solicitudes de registro e inicio de sesión simultáneamente sin degradar el rendimiento.
3. **Escalabilidad:** El sistema debe ser escalable para soportar un creciente número de usuarios y datos.
4. **Usabilidad:** La interfaz de usuario debe ser intuitiva y fácil de usar, permitiendo a los usuarios registrarse e iniciar sesión sin dificultades.
5. **Mantenibilidad:** El código del modelo de usuario debe ser claro y bien documentado para facilitar su mantenimiento y futuras modificaciones.

## 3. Implementación en Python

### a. Explicación paso a paso del código

Para implementar un modelo de usuario personalizado en Django, seguiremos estos pasos:

1. **Crear una nueva aplicación:** Primero, crearemos una nueva aplicación en nuestro proyecto Django para gestionar el modelo de usuario.
2. **Extender `AbstractUser`:** Crearemos un nuevo modelo que extienda `AbstractUser` y añadiremos los campos personalizados.
3. **Configurar el modelo de usuario en `settings.py`:** Configuraremos el proyecto Django para utilizar nuestro modelo de usuario personalizado.
4. **Migraciones:** Crearemos y aplicaremos las migraciones necesarias para actualizar la base de datos con el nuevo modelo de usuario.
5. **Formularios de registro e inicio de sesión:** Crearemos formularios personalizados para el registro e inicio de sesión de usuarios.
6. **Vistas y URLs:** Implementaremos las vistas y URLs necesarias para gestionar el registro e inicio de sesión de usuarios.

### b. Código fuente completo y comentado

#### Paso 1: Crear una nueva aplicación

```bash
python manage.py startapp usuarios
```

#### Paso 2: Extender `AbstractUser`

En `usuarios/models.py`:

```python
from django.contrib.auth.models import AbstractUser
from django.db import models

class Usuario(AbstractUser):
    direccion_envio = models.CharField(max_length=255, blank=True, null=True)
    telefono = models.CharField(max_length=15, blank=True, null=True)

    def __str__(self):
        return self.username
```

#### Paso 3: Configurar el modelo de usuario en `settings.py`

En `settings.py`:

```python
# Añadir la aplicación 'usuarios' a INSTALLED_APPS
INSTALLED_APPS = [
    ...
    'usuarios',
    ...
]

# Configurar el modelo de usuario personalizado
AUTH_USER_MODEL = 'usuarios.Usuario'
```

#### Paso 4: Migraciones

```bash
python manage.py makemigrations usuarios
python manage.py migrate
```

#### Paso 5: Formularios de registro e inicio de sesión

En `usuarios/forms.py`:

```python
from django import forms
from django.contrib.auth.forms import UserCreationForm, UserChangeForm
from .models import Usuario

class RegistroForm(UserCreationForm):
    class Meta:
        model = Usuario
        fields = ('username', 'email', 'direccion_envio', 'telefono', 'password1', 'password2')

class ActualizarUsuarioForm(UserChangeForm):
    class Meta:
        model = Usuario
        fields = ('username', 'email', 'direccion_envio', 'telefono')
```

#### Paso 6: Vistas y URLs

En `usuarios/views.py`:

```python
from django.urls import reverse_lazy
from django.views import generic
from .forms import RegistroForm

class RegistroView(generic.CreateView):
    form_class = RegistroForm
    success_url = reverse_lazy('login')
    template_name = 'usuarios/registro.html'
```

En `usuarios/urls.py`:

```python
from django.urls import path
from .views import RegistroView

urlpatterns = [
    path('registro/', RegistroView.as_view(), name='registro'),
]
```

En `urls.py` del proyecto principal:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('usuarios/', include('usuarios.urls')),
    path('accounts/', include('django.contrib.auth.urls')),
]
```

En `usuarios/templates/usuarios/registro.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Registro</title>
</head>
<body>
    <h2>Registro de Usuario</h2>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Registrarse</button>
    </form>
</body>
</html>
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

1. **Registro de usuario:**
   - El usuario accede a `http://localhost:8000/usuarios/registro/`.
   - Completa el formulario de registro con su información personal.
   - El sistema crea una nueva cuenta de usuario y redirige al usuario a la página de inicio de sesión.

2. **Inicio de sesión de usuario:**
   - El usuario accede a `http://localhost:8000/accounts/login/`.
   - Ingresa su nombre de usuario y contraseña.
   - El sistema autentica al usuario y lo redirige a la página principal.

#### Pruebas unitarias

En `usuarios/tests.py`:

```python
from django.test import TestCase
from django.urls import reverse
from .models import Usuario

class UsuarioTests(TestCase):

    def test_creacion_usuario(self):
        usuario = Usuario.objects.create_user(
            username='testuser',
            email='testuser@example.com',
            password='testpassword123',
            direccion_envio='123 Calle Falsa',
            telefono='123456789'
        )
        self.assertEqual(usuario.username, 'testuser')
        self.assertEqual(usuario.email, 'testuser@example.com')
        self.assertTrue(usuario.check_password('testpassword123'))
        self.assertEqual(usuario.direccion_envio, '123 Calle Falsa')
        self.assertEqual(usuario.telefono, '123456789')

    def test_registro_usuario(self):
        response = self.client.post(reverse('registro'), {
            'username': 'testuser',
            'email': 'testuser@example.com',
            'password1': 'testpassword123',
            'password2': 'testpassword123',
            'direccion_envio': '123 Calle Falsa',
            'telefono': '123456789'
        })
        self.assertEqual(response.status_code, 302)
        usuario = Usuario.objects.get(username='testuser')
        self.assertEqual(usuario.email, 'testuser@example.com')
        self.assertEqual(usuario.direccion_envio, '123 Calle Falsa')
        self.assertEqual(usuario.telefono, '123456789')
```

## 4. Mejores prácticas y consideraciones de diseño

### Mejores prácticas

1. **Seguridad:** Asegúrate de que las contraseñas de los usuarios estén encriptadas utilizando los mecanismos de seguridad proporcionados por Django.
2. **Validación de datos:** Implementa validaciones adecuadas en los formularios para asegurar que los datos ingresados por los usuarios sean correctos y completos.
3. **Pruebas:** Realiza pruebas unitarias y de integración para asegurar el correcto funcionamiento del modelo de usuario y las funcionalidades relacionadas.
4. **Documentación:** Documenta el código y las funcionalidades del modelo de usuario para facilitar su mantenimiento y futuras modificaciones.
5. **Modularidad:** Mantén el código modular y organizado, separando las funcionalidades en diferentes archivos y aplicaciones según sea necesario.

### Consideraciones de diseño

1. **Extensibilidad:** Diseña el modelo de usuario de manera que sea fácil de extender y personalizar en el futuro.
2. **Reutilización:** Aprovecha las funcionalidades y componentes proporcionados por Django, como los formularios y vistas genéricas, para reducir la cantidad de código personalizado.
3. **Escalabilidad:** Considera el impacto del modelo de usuario en el rendimiento y escalabilidad del sistema, especialmente si se espera un gran número de usuarios.
4. **Usabilidad:** Diseña la interfaz de usuario de manera que sea intuitiva y fácil de usar, facilitando el registro e inicio de sesión de los usuarios.

### Consejos para la depuración y solución de problemas comunes

1. **Errores de migración:** Si encuentras errores al aplicar las migraciones, asegúrate de que el modelo de usuario esté correctamente configurado en `settings.py` y que todas las dependencias estén instaladas.
2. **Problemas de autenticación:** Si los usuarios no pueden iniciar sesión, verifica que las credenciales sean correctas y que el sistema de autenticación esté configurado adecuadamente.
3. **Validación de formularios:** Si los formularios no se validan correctamente, revisa las reglas de validación y asegúrate de que los datos ingresados cumplan con los requisitos.
4. **Mensajes de error:** Proporciona mensajes de error claros y útiles para ayudar a los usuarios a corregir cualquier problema que encuentren durante el registro o inicio de sesión.

Con esto, hemos cubierto de manera exhaustiva la creación y personalización de un modelo de usuario en Django mediante la extensión de `AbstractUser`, así como su integración en un sistema de carrito de compras. Siguiendo estos pasos y mejores prácticas, podrás implementar un sistema de autenticación y gestión de usuarios robusto y escalable en tu proyecto Django.

# Unidad: Modelo de Producto en Django

## Introducción a la unidad y objetivos de aprendizaje

Bienvenidos a esta unidad del manual "Sistema de Carrito de Compras con Django". En esta sección, nos enfocaremos en el "Modelo de Producto". Este modelo es crucial para cualquier sistema de comercio electrónico, ya que representa los artículos que los usuarios pueden comprar. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender la estructura y los componentes del modelo de producto en Django.
2. Implementar un modelo de producto con atributos esenciales.
3. Utilizar técnicas avanzadas para mejorar la funcionalidad y el rendimiento del modelo.
4. Aplicar mejores prácticas en el diseño y la implementación del modelo de producto.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

El modelo de producto en un sistema de carrito de compras debe ser capaz de representar todos los detalles relevantes de un producto que se ofrece a la venta. Esto incluye atributos básicos como el nombre, la descripción, el precio y la disponibilidad, así como atributos más avanzados como categorías, imágenes y variantes de productos.

### Casos de uso

1. **Crear un producto**: Un administrador puede crear un nuevo producto ingresando detalles como nombre, descripción, precio, categoría e imágenes.
2. **Actualizar un producto**: Un administrador puede actualizar los detalles de un producto existente.
3. **Eliminar un producto**: Un administrador puede eliminar un producto del catálogo.
4. **Listar productos**: Los usuarios pueden ver una lista de productos disponibles.
5. **Buscar productos**: Los usuarios pueden buscar productos por nombre, categoría u otros atributos.
6. **Ver detalles de un producto**: Los usuarios pueden ver los detalles completos de un producto específico.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Crear Producto]
    A --> C[Actualizar Producto]
    A --> D[Eliminar Producto]
    A --> E[Listar Productos]
    A --> F[Buscar Productos]
    A --> G[Ver Detalles de Producto]
    
    B --> H[Ingresar Detalles del Producto]
    H --> I[Guardar Producto en la Base de Datos]
    I --> J[Producto Creado]

    C --> K[Seleccionar Producto a Actualizar]
    K --> L[Modificar Detalles del Producto]
    L --> M[Guardar Cambios en la Base de Datos]
    M --> N[Producto Actualizado]

    D --> O[Seleccionar Producto a Eliminar]
    O --> P[Confirmar Eliminación]
    P --> Q[Eliminar Producto de la Base de Datos]
    Q --> R[Producto Eliminado]

    E --> S[Consultar Base de Datos]
    S --> T[Mostrar Lista de Productos]

    F --> U[Ingresar Criterios de Búsqueda]
    U --> V[Consultar Base de Datos]
    V --> W[Mostrar Resultados de Búsqueda]

    G --> X[Seleccionar Producto]
    X --> Y[Mostrar Detalles del Producto]
```

### Requisitos no funcionales

1. **Rendimiento**: La consulta y visualización de productos debe ser rápida y eficiente, incluso con una gran cantidad de productos en la base de datos.
2. **Escalabilidad**: El sistema debe ser capaz de manejar un crecimiento en el número de productos sin degradar el rendimiento.
3. **Seguridad**: Solo los administradores autorizados deben poder crear, actualizar o eliminar productos.
4. **Mantenibilidad**: El código debe ser claro y bien documentado para facilitar futuras modificaciones y mejoras.
5. **Usabilidad**: La interfaz de usuario para la gestión de productos debe ser intuitiva y fácil de usar.

## Implementación en Python

### Explicación paso a paso del código

Para implementar el modelo de producto en Django, seguiremos estos pasos:

1. **Definir el modelo de producto**: Crearemos una clase `Product` en el archivo `models.py` que heredará de `models.Model`.
2. **Agregar atributos básicos**: Añadiremos campos como `name`, `description`, `price`, `stock`, `category` e `image`.
3. **Configurar las relaciones**: Definiremos relaciones con otros modelos, como `Category`.
4. **Implementar métodos adicionales**: Añadiremos métodos para calcular descuentos, verificar disponibilidad, etc.
5. **Registrar el modelo en el administrador de Django**: Permitiremos que los administradores gestionen productos desde el panel de administración.

### Código fuente completo y comentado

```python
from django.db import models

class Category(models.Model):
    name = models.CharField(max_length=255)
    description = models.TextField(blank=True, null=True)

    def __str__(self):
        return self.name

class Product(models.Model):
    name = models.CharField(max_length=255)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField()
    category = models.ForeignKey(Category, related_name='products', on_delete=models.CASCADE)
    image = models.ImageField(upload_to='products/', blank=True, null=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.name

    def is_in_stock(self):
        return self.stock > 0

    def apply_discount(self, percentage):
        discount_amount = self.price * (percentage / 100)
        return self.price - discount_amount

# Registro del modelo en el administrador de Django
from django.contrib import admin

@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    list_display = ('name', 'price', 'stock', 'category', 'created_at', 'updated_at')
    search_fields = ('name', 'description')
    list_filter = ('category', 'created_at', 'updated_at')
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplos de uso

1. **Crear un producto**:
   ```python
   from myapp.models import Product, Category

   electronics = Category.objects.create(name='Electronics', description='Electronic items')
   product = Product.objects.create(
       name='Smartphone',
       description='A high-end smartphone with 128GB storage',
       price=699.99,
       stock=50,
       category=electronics
   )
   ```

2. **Actualizar un producto**:
   ```python
   product = Product.objects.get(id=1)
   product.price = 649.99
   product.save()
   ```

3. **Eliminar un producto**:
   ```python
   product = Product.objects.get(id=1)
   product.delete()
   ```

4. **Listar productos**:
   ```python
   products = Product.objects.all()
   for product in products:
       print(product.name, product.price)
   ```

5. **Buscar productos**:
   ```python
   products = Product.objects.filter(name__icontains='smartphone')
   for product in products:
       print(product.name, product.price)
   ```

6. **Verificar disponibilidad**:
   ```python
   product = Product.objects.get(id=1)
   if product.is_in_stock():
       print(f'{product.name} is in stock')
   else:
       print(f'{product.name} is out of stock')
   ```

#### Pruebas unitarias

```python
from django.test import TestCase
from myapp.models import Product, Category

class ProductModelTest(TestCase):

    def setUp(self):
        self.category = Category.objects.create(name='Electronics', description='Electronic items')
        self.product = Product.objects.create(
            name='Smartphone',
            description='A high-end smartphone with 128GB storage',
            price=699.99,
            stock=50,
            category=self.category
        )

    def test_product_creation(self):
        self.assertEqual(self.product.name, 'Smartphone')
        self.assertEqual(self.product.price, 699.99)
        self.assertEqual(self.product.stock, 50)
        self.assertEqual(self.product.category.name, 'Electronics')

    def test_product_is_in_stock(self):
        self.assertTrue(self.product.is_in_stock())

    def test_product_apply_discount(self):
        discounted_price = self.product.apply_discount(10)
        self.assertEqual(discounted_price, 629.99)
```

## Mejores prácticas y consideraciones de diseño

1. **Normalización de la base de datos**: Asegúrate de que los datos estén bien normalizados. Por ejemplo, utiliza una tabla separada para las categorías en lugar de almacenar la categoría como un campo de texto en el modelo de producto.
2. **Validación de datos**: Implementa validaciones en los modelos para asegurar que los datos ingresados sean correctos. Por ejemplo, verifica que el precio no sea negativo.
3. **Optimización de consultas**: Utiliza `select_related` y `prefetch_related` para optimizar las consultas a la base de datos y evitar problemas de rendimiento.
4. **Seguridad**: Asegúrate de que solo los usuarios autorizados puedan realizar operaciones sensibles como la creación, actualización y eliminación de productos.
5. **Documentación**: Mantén tu código bien documentado. Utiliza docstrings y comentarios para explicar la lógica y el propósito de cada parte del código.
6. **Pruebas**: Escribe pruebas unitarias y de integración para asegurarte de que tu modelo de producto funcione correctamente y para facilitar la detección de errores.
7. **Escalabilidad**: Diseña tu modelo pensando en el futuro. Considera cómo manejarás un gran número de productos y cómo escalarás tu aplicación a medida que crezca.

En resumen, el modelo de producto es una parte fundamental de cualquier sistema de carrito de compras. Siguiendo las mejores prácticas y consideraciones de diseño, puedes crear un modelo robusto y eficiente que satisfaga las necesidades de tu aplicación y proporcione una experiencia de usuario excelente.

# Unidad: Modelo de Carrito y CarritoItem

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, profundizaremos en los modelos de Django para la creación de un sistema de carrito de compras, específicamente en los modelos `Carrito` y `CarritoItem`. Estos modelos son fundamentales para gestionar la lógica de un carrito de compras en una aplicación web. Al finalizar esta unidad, serás capaz de:

1. Comprender la estructura y funcionalidad de los modelos `Carrito` y `CarritoItem`.
2. Implementar estos modelos en Django de manera eficiente.
3. Crear y manipular instancias de estos modelos para gestionar un carrito de compras.
4. Aplicar mejores prácticas de diseño y considerar aspectos de seguridad, rendimiento y escalabilidad.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

El modelo `Carrito` representa un carrito de compras, que puede contener múltiples productos. Cada producto en el carrito se representa mediante una instancia del modelo `CarritoItem`. Un `CarritoItem` contiene información sobre el producto específico y la cantidad de dicho producto en el carrito.

#### Modelo Carrito
- **Atributos**:
  - `usuario`: Referencia al modelo de usuario que posee el carrito.
  - `fecha_creacion`: Fecha y hora en que se creó el carrito.
  - `activo`: Indica si el carrito está activo o no.

#### Modelo CarritoItem
- **Atributos**:
  - `carrito`: Referencia al carrito al que pertenece el ítem.
  - `producto`: Referencia al modelo de producto.
  - `cantidad`: Cantidad del producto en el carrito.
  - `precio`: Precio del producto en el momento de añadirlo al carrito.

### b. Casos de uso

1. **Añadir un producto al carrito**:
   - Un usuario selecciona un producto y lo añade a su carrito.
   - Si el producto ya está en el carrito, se incrementa la cantidad.

2. **Eliminar un producto del carrito**:
   - Un usuario decide eliminar un producto de su carrito.
   - El ítem correspondiente se elimina del carrito.

3. **Actualizar la cantidad de un producto en el carrito**:
   - Un usuario modifica la cantidad de un producto en su carrito.
   - La cantidad del ítem correspondiente se actualiza.

4. **Visualizar el contenido del carrito**:
   - Un usuario desea ver los productos que ha añadido a su carrito.
   - Se muestra una lista de todos los ítems en el carrito, junto con sus detalles.

### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Seleccionar Producto]
    B --> C{Producto ya en Carrito?}
    C -->|Sí| D[Incrementar Cantidad]
    C -->|No| E[Añadir Producto al Carrito]
    D --> F[Actualizar Carrito]
    E --> F
    F --> G[Fin]

    H[Inicio] --> I[Seleccionar Producto en Carrito]
    I --> J[Eliminar Producto del Carrito]
    J --> K[Actualizar Carrito]
    K --> L[Fin]

    M[Inicio] --> N[Seleccionar Producto en Carrito]
    N --> O[Modificar Cantidad]
    O --> P[Actualizar Carrito]
    P --> Q[Fin]

    R[Inicio] --> S[Ver Carrito]
    S --> T[Mostrar Contenido del Carrito]
    T --> U[Fin]
```

### d. Requisitos no funcionales

1. **Rendimiento**:
   - Las operaciones de añadir, eliminar y actualizar productos en el carrito deben ser rápidas y eficientes.
   - El sistema debe ser capaz de manejar múltiples carritos simultáneamente sin degradar el rendimiento.

2. **Escalabilidad**:
   - El diseño debe permitir la escalabilidad horizontal, permitiendo que el sistema maneje un número creciente de usuarios y carritos.

3. **Seguridad**:
   - Solo el usuario propietario del carrito debe poder modificar su contenido.
   - Las operaciones deben ser seguras contra ataques de inyección SQL y otros tipos de vulnerabilidades.

4. **Usabilidad**:
   - La interfaz de usuario debe ser intuitiva y fácil de usar.
   - Los mensajes de error y confirmación deben ser claros y útiles.

## Implementación en Python

### a. Explicación paso a paso del código

Para implementar los modelos `Carrito` y `CarritoItem` en Django, seguiremos los siguientes pasos:

1. **Definir el modelo `Carrito`**:
   - Este modelo contendrá una referencia al usuario, la fecha de creación y un campo booleano para indicar si el carrito está activo.

2. **Definir el modelo `CarritoItem`**:
   - Este modelo contendrá una referencia al carrito, una referencia al producto, la cantidad y el precio del producto.

3. **Registrar los modelos en el administrador de Django**:
   - Esto permitirá gestionar los carritos y sus ítems desde la interfaz de administración de Django.

### b. Código fuente completo y comentado

```python
from django.db import models
from django.conf import settings
from productos.models import Producto

class Carrito(models.Model):
    usuario = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    fecha_creacion = models.DateTimeField(auto_now_add=True)
    activo = models.BooleanField(default=True)

    def __str__(self):
        return f"Carrito de {self.usuario.username} - {self.fecha_creacion}"

    def total(self):
        return sum(item.subtotal() for item in self.items.all())

class CarritoItem(models.Model):
    carrito = models.ForeignKey(Carrito, related_name='items', on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.PositiveIntegerField(default=1)
    precio = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return f"{self.cantidad} x {self.producto.nombre}"

    def subtotal(self):
        return self.cantidad * self.precio

# Registrar los modelos en el administrador de Django
from django.contrib import admin

@admin.register(Carrito)
class CarritoAdmin(admin.ModelAdmin):
    list_display = ('usuario', 'fecha_creacion', 'activo')

@admin.register(CarritoItem)
class CarritoItemAdmin(admin.ModelAdmin):
    list_display = ('carrito', 'producto', 'cantidad', 'precio')
```

### c. Ejemplos de uso y pruebas unitarias

#### Añadir un producto al carrito

```python
from django.test import TestCase
from django.contrib.auth.models import User
from productos.models import Producto
from .models import Carrito, CarritoItem

class CarritoTestCase(TestCase):
    def setUp(self):
        self.usuario = User.objects.create_user(username='testuser', password='12345')
        self.producto = Producto.objects.create(nombre='Producto 1', precio=100.00)
        self.carrito = Carrito.objects.create(usuario=self.usuario)

    def test_añadir_producto_al_carrito(self):
        item = CarritoItem.objects.create(carrito=self.carrito, producto=self.producto, cantidad=2, precio=self.producto.precio)
        self.assertEqual(self.carrito.items.count(), 1)
        self.assertEqual(item.subtotal(), 200.00)
```

#### Eliminar un producto del carrito

```python
    def test_eliminar_producto_del_carrito(self):
        item = CarritoItem.objects.create(carrito=self.carrito, producto=self.producto, cantidad=2, precio=self.producto.precio)
        item.delete()
        self.assertEqual(self.carrito.items.count(), 0)
```

#### Actualizar la cantidad de un producto en el carrito

```python
    def test_actualizar_cantidad_producto_en_carrito(self):
        item = CarritoItem.objects.create(carrito=self.carrito, producto=self.producto, cantidad=2, precio=self.producto.precio)
        item.cantidad = 5
        item.save()
        self.assertEqual(item.cantidad, 5)
        self.assertEqual(item.subtotal(), 500.00)
```

#### Visualizar el contenido del carrito

```python
    def test_visualizar_contenido_del_carrito(self):
        item1 = CarritoItem.objects.create(carrito=self.carrito, producto=self.producto, cantidad=2, precio=self.producto.precio)
        producto2 = Producto.objects.create(nombre='Producto 2', precio=50.00)
        item2 = CarritoItem.objects.create(carrito=self.carrito, producto=producto2, cantidad=3, precio=producto2.precio)
        self.assertEqual(self.carrito.items.count(), 2)
        self.assertEqual(self.carrito.total(), 350.00)
```

## Mejores prácticas y consideraciones de diseño

1. **Uso de señales de Django**:
   - Considera el uso de señales para realizar acciones automáticas, como actualizar el total del carrito cuando se añade o elimina un ítem.

2. **Validaciones y restricciones**:
   - Implementa validaciones para asegurar que la cantidad de productos no sea negativa y que el precio sea válido.

3. **Optimización de consultas**:
   - Utiliza `select_related` y `prefetch_related` para optimizar las consultas a la base de datos y reducir el número de consultas necesarias.

4. **Seguridad**:
   - Asegúrate de que solo el usuario propietario del carrito pueda modificar su contenido.
   - Utiliza transacciones para asegurar la consistencia de los datos en operaciones críticas.

5. **Escalabilidad**:
   - Diseña el sistema para soportar un gran número de usuarios y carritos, utilizando técnicas como el particionamiento de bases de datos si es necesario.

6. **Pruebas exhaustivas**:
   - Implementa pruebas unitarias y de integración para asegurar que todas las funcionalidades del carrito funcionan correctamente.

7. **Documentación**:
   - Documenta el código y las funcionalidades para facilitar el mantenimiento y la extensión del sistema.

En resumen, los modelos `Carrito` y `CarritoItem` son componentes esenciales en la implementación de un sistema de carrito de compras en Django. Siguiendo las mejores prácticas y consideraciones de diseño, puedes crear un sistema robusto, eficiente y seguro que brinde una excelente experiencia de usuario.

# Unidad: Modelo de Orden y OrderItem

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, profundizaremos en el desarrollo del Modelo de Orden y OrderItem en Django, parte esencial del capítulo "Modelos de Django" del libro "Sistema de Carrito de Compras con Django". El objetivo es proporcionar una comprensión exhaustiva de cómo estos modelos interactúan con otros componentes del sistema, como el Modelo de Usuario, Producto, Carrito y CarritoItem, y cómo se implementan en Django.

Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender la estructura y funcionalidad del Modelo de Orden y OrderItem.
2. Implementar estos modelos en Django de manera eficiente.
3. Integrar estos modelos con otros componentes del sistema de carrito de compras.
4. Aplicar mejores prácticas en el diseño y desarrollo de modelos en Django.
5. Realizar pruebas unitarias y asegurar la calidad del código.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El Modelo de Orden y OrderItem es crucial para gestionar las compras realizadas por los usuarios en el sistema de carrito de compras. La Orden representa una compra completa realizada por un usuario, mientras que OrderItem representa cada producto individual dentro de esa compra.

#### Modelo de Orden

- **ID**: Identificador único de la orden.
- **Usuario**: Referencia al usuario que realizó la orden.
- **Fecha de creación**: Fecha y hora en que se creó la orden.
- **Estado**: Estado actual de la orden (pendiente, procesada, enviada, entregada).
- **Total**: Monto total de la orden.

#### Modelo de OrderItem

- **ID**: Identificador único del item de la orden.
- **Orden**: Referencia a la orden a la que pertenece el item.
- **Producto**: Referencia al producto comprado.
- **Cantidad**: Cantidad del producto comprado.
- **Precio**: Precio del producto en el momento de la compra.

### Casos de Uso

1. **Creación de una Orden**: Un usuario finaliza su compra y se crea una nueva orden con los items correspondientes.
2. **Actualización del Estado de la Orden**: El administrador del sistema actualiza el estado de una orden (por ejemplo, de pendiente a procesada).
3. **Visualización de Historial de Órdenes**: Un usuario visualiza el historial de sus órdenes pasadas.
4. **Cálculo del Total de la Orden**: El sistema calcula el monto total de una orden sumando los precios de todos los items.

### Diagramas de Flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Usuario finaliza compra]
    B --> C[Crear Orden]
    C --> D[Agregar OrderItems]
    D --> E[Calcular Total de la Orden]
    E --> F[Guardar Orden en la Base de Datos]
    F --> G[Fin]

    H[Administrador actualiza estado de la orden] --> I[Seleccionar Orden]
    I --> J[Actualizar Estado]
    J --> K[Guardar Cambios]
    K --> L[Fin]
```

### Requisitos No Funcionales

1. **Rendimiento**: El sistema debe ser capaz de manejar múltiples órdenes simultáneamente sin degradar el rendimiento.
2. **Escalabilidad**: La arquitectura debe permitir la fácil adición de nuevas funcionalidades relacionadas con las órdenes.
3. **Seguridad**: Los datos de las órdenes deben estar protegidos y solo accesibles por usuarios autorizados.
4. **Mantenibilidad**: El código debe ser claro y bien documentado para facilitar futuras modificaciones.

## Implementación en Python

### Explicación Paso a Paso del Código

1. **Definición del Modelo de Orden**: Se define una clase `Order` que hereda de `models.Model`. Esta clase contiene campos para almacenar la información de la orden, como el usuario, la fecha de creación, el estado y el total.

2. **Definición del Modelo de OrderItem**: Se define una clase `OrderItem` que también hereda de `models.Model`. Esta clase contiene campos para almacenar la información de cada item de la orden, como la referencia a la orden, el producto, la cantidad y el precio.

3. **Relaciones entre Modelos**: Se utilizan claves foráneas (`ForeignKey`) para establecer relaciones entre `Order`, `OrderItem`, `User` y `Product`.

4. **Métodos Personalizados**: Se implementan métodos personalizados en los modelos para calcular el total de la orden y actualizar el estado.

### Código Fuente Completo y Comentado

```python
from django.db import models
from django.contrib.auth.models import User
from productos.models import Producto

class Order(models.Model):
    PENDIENTE = 'P'
    PROCESADA = 'PR'
    ENVIADA = 'E'
    ENTREGADA = 'EN'

    ESTADO_CHOICES = [
        (PENDIENTE, 'Pendiente'),
        (PROCESADA, 'Procesada'),
        (ENVIADA, 'Enviada'),
        (ENTREGADA, 'Entregada'),
    ]

    usuario = models.ForeignKey(User, on_delete=models.CASCADE)
    fecha_creacion = models.DateTimeField(auto_now_add=True)
    estado = models.CharField(max_length=2, choices=ESTADO_CHOICES, default=PENDIENTE)
    total = models.DecimalField(max_digits=10, decimal_places=2, default=0.00)

    def __str__(self):
        return f'Orden {self.id} - Usuario: {self.usuario.username}'

    def calcular_total(self):
        self.total = sum(item.precio * item.cantidad for item in self.orderitem_set.all())
        self.save()

class OrderItem(models.Model):
    orden = models.ForeignKey(Order, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.PositiveIntegerField(default=1)
    precio = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return f'{self.cantidad} x {self.producto.nombre}'

    def save(self, *args, **kwargs):
        self.precio = self.producto.precio
        super().save(*args, **kwargs)
        self.orden.calcular_total()
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

```python
# Crear una nueva orden
usuario = User.objects.get(username='usuario1')
orden = Order.objects.create(usuario=usuario)

# Agregar items a la orden
producto1 = Producto.objects.get(nombre='Producto 1')
producto2 = Producto.objects.get(nombre='Producto 2')

order_item1 = OrderItem.objects.create(orden=orden, producto=producto1, cantidad=2)
order_item2 = OrderItem.objects.create(orden=orden, producto=producto2, cantidad=1)

# Verificar el total de la orden
print(orden.total)  # Debería mostrar la suma de los precios de los productos
```

#### Pruebas Unitarias

```python
from django.test import TestCase
from django.contrib.auth.models import User
from productos.models import Producto
from .models import Order, OrderItem

class OrderModelTest(TestCase):

    def setUp(self):
        self.usuario = User.objects.create_user(username='testuser', password='12345')
        self.producto1 = Producto.objects.create(nombre='Producto 1', precio=100.00)
        self.producto2 = Producto.objects.create(nombre='Producto 2', precio=200.00)
        self.orden = Order.objects.create(usuario=self.usuario)

    def test_crear_order_item(self):
        order_item = OrderItem.objects.create(orden=self.orden, producto=self.producto1, cantidad=2)
        self.assertEqual(order_item.precio, self.producto1.precio)
        self.assertEqual(self.orden.total, 200.00)

    def test_calcular_total(self):
        OrderItem.objects.create(orden=self.orden, producto=self.producto1, cantidad=2)
        OrderItem.objects.create(orden=self.orden, producto=self.producto2, cantidad=1)
        self.orden.calcular_total()
        self.assertEqual(self.orden.total, 400.00)

    def test_actualizar_estado(self):
        self.orden.estado = Order.PROCESADA
        self.orden.save()
        self.assertEqual(self.orden.estado, Order.PROCESADA)
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Normalización de la Base de Datos**: Asegúrese de que los modelos estén correctamente normalizados para evitar redundancias y mejorar la eficiencia de las consultas.
2. **Uso de Métodos Personalizados**: Implementar métodos personalizados en los modelos para encapsular la lógica de negocio, como el cálculo del total de la orden.
3. **Validaciones y Restricciones**: Utilizar validaciones y restricciones en los modelos para asegurar la integridad de los datos (por ejemplo, cantidad positiva en OrderItem).
4. **Optimización de Consultas**: Utilizar técnicas de optimización de consultas, como `select_related` y `prefetch_related`, para mejorar el rendimiento al acceder a relaciones entre modelos.
5. **Seguridad**: Asegurar que solo los usuarios autorizados puedan acceder y modificar las órdenes. Utilizar permisos y autenticación adecuados.
6. **Pruebas Unitarias**: Escribir pruebas unitarias para validar la funcionalidad de los modelos y asegurar la calidad del código.
7. **Documentación**: Mantener una buena documentación del código para facilitar el mantenimiento y la comprensión por parte de otros desarrolladores.

En resumen, el desarrollo del Modelo de Orden y OrderItem en Django es un componente fundamental para cualquier sistema de carrito de compras. Siguiendo las mejores prácticas y consideraciones de diseño, se puede asegurar una implementación eficiente, segura y escalable.

# Unidad: Modelo de Categoría y Modelos Relacionados

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, exploraremos en profundidad el modelo de Categoría y los modelos relacionados dentro del contexto de un sistema de carrito de compras desarrollado con Django. Este modelo es crucial para organizar y gestionar los productos de manera eficiente, permitiendo a los usuarios navegar y encontrar productos de manera intuitiva. Al finalizar esta unidad, serás capaz de:

1. Comprender la importancia del modelo de Categoría en un sistema de carrito de compras.
2. Diseñar y crear modelos relacionados con Categoría en Django.
3. Implementar relaciones entre modelos y comprender su impacto en la base de datos.
4. Aplicar mejores prácticas en el diseño y desarrollo de modelos en Django.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El modelo de Categoría es fundamental para la organización de productos en un sistema de carrito de compras. Este modelo permite clasificar los productos en diferentes categorías, facilitando la navegación y búsqueda para los usuarios. Además, se pueden establecer relaciones jerárquicas entre categorías (por ejemplo, subcategorías), lo que permite una estructura más detallada y organizada.

### Casos de Uso

1. **Administrador del Sistema**:
   - Crear, editar y eliminar categorías y subcategorías.
   - Asignar productos a categorías específicas.
   - Visualizar la jerarquía de categorías y subcategorías.

2. **Usuario Final**:
   - Navegar por las categorías para encontrar productos.
   - Filtrar productos por categoría.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Administrador inicia sesión]
    B --> C[Accede a la sección de categorías]
    C --> D[Crear nueva categoría]
    C --> E[Editar categoría existente]
    C --> F[Eliminar categoría]
    D --> G[Guardar categoría]
    E --> G[Guardar cambios]
    F --> H[Confirmar eliminación]
    G --> I[Categoría actualizada]
    H --> I
    I --> J[Fin]
```

### Requisitos No Funcionales

1. **Rendimiento**: El sistema debe ser capaz de manejar un gran número de categorías y subcategorías sin afectar el rendimiento.
2. **Escalabilidad**: La estructura de categorías debe ser escalable para permitir la adición de nuevas categorías y subcategorías sin necesidad de cambios significativos en la base de datos.
3. **Seguridad**: Solo los administradores autorizados deben tener acceso a la funcionalidad de gestión de categorías.
4. **Usabilidad**: La interfaz de usuario para la gestión de categorías debe ser intuitiva y fácil de usar.

## Implementación en Python

### Explicación Paso a Paso del Código

1. **Definición del Modelo de Categoría**: Crearemos un modelo `Category` que tendrá campos como `name` y `parent` para representar la jerarquía de categorías.
2. **Relaciones entre Modelos**: Estableceremos relaciones entre `Category` y otros modelos como `Product`.
3. **Admin Interface**: Configuraremos la interfaz de administración de Django para gestionar las categorías.

### Código Fuente Completo y Comentado

```python
from django.db import models

class Category(models.Model):
    name = models.CharField(max_length=255, unique=True)
    parent = models.ForeignKey('self', on_delete=models.CASCADE, null=True, blank=True, related_name='subcategories')

    class Meta:
        verbose_name_plural = "Categories"

    def __str__(self):
        return self.name

class Product(models.Model):
    name = models.CharField(max_length=255)
    category = models.ForeignKey(Category, on_delete=models.CASCADE, related_name='products')
    price = models.DecimalField(max_digits=10, decimal_places=2)
    description = models.TextField()

    def __str__(self):
        return self.name
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

```python
# Crear una categoría principal
electronics = Category.objects.create(name="Electronics")

# Crear una subcategoría
smartphones = Category.objects.create(name="Smartphones", parent=electronics)

# Crear un producto y asignarlo a una categoría
iphone = Product.objects.create(name="iPhone 12", category=smartphones, price=999.99, description="Latest Apple iPhone")
```

#### Pruebas Unitarias

```python
from django.test import TestCase
from .models import Category, Product

class CategoryModelTest(TestCase):

    def setUp(self):
        self.electronics = Category.objects.create(name="Electronics")
        self.smartphones = Category.objects.create(name="Smartphones", parent=self.electronics)
        self.iphone = Product.objects.create(name="iPhone 12", category=self.smartphones, price=999.99, description="Latest Apple iPhone")

    def test_category_creation(self):
        self.assertEqual(self.electronics.name, "Electronics")
        self.assertEqual(self.smartphones.parent, self.electronics)

    def test_product_creation(self):
        self.assertEqual(self.iphone.name, "iPhone 12")
        self.assertEqual(self.iphone.category, self.smartphones)
        self.assertEqual(self.iphone.price, 999.99)
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Normalización de la Base de Datos**: Asegúrate de que la base de datos esté normalizada para evitar redundancias y mejorar la integridad de los datos.
2. **Uso de ForeignKey**: Utiliza `ForeignKey` para establecer relaciones entre modelos, lo que facilita la gestión y consulta de datos relacionados.
3. **Optimización de Consultas**: Utiliza `select_related` y `prefetch_related` para optimizar las consultas y mejorar el rendimiento.
4. **Seguridad**: Implementa permisos y autenticación adecuados para asegurar que solo los usuarios autorizados puedan gestionar las categorías.
5. **Escalabilidad**: Diseña la estructura de categorías de manera que pueda escalar fácilmente con el crecimiento del catálogo de productos.

### Consideraciones de Seguridad

- **Autenticación y Autorización**: Implementa un sistema robusto de autenticación y autorización para asegurar que solo los administradores puedan gestionar las categorías.
- **Validación de Datos**: Asegúrate de validar todos los datos de entrada para prevenir ataques de inyección SQL y otros tipos de vulnerabilidades.

### Consideraciones de Rendimiento

- **Índices en la Base de Datos**: Crea índices en los campos que se utilizan frecuentemente en consultas para mejorar el rendimiento.
- **Caching**: Implementa estrategias de caching para reducir la carga en la base de datos y mejorar la velocidad de respuesta.

### Consejos para la Depuración y Solución de Problemas Comunes

1. **Errores de Migración**: Si encuentras errores al aplicar migraciones, revisa las dependencias entre modelos y asegúrate de que todas las migraciones anteriores se hayan aplicado correctamente.
2. **Problemas de Rendimiento**: Utiliza herramientas de profiling para identificar consultas lentas y optimizarlas.
3. **Errores de Relación**: Si encuentras errores relacionados con las relaciones entre modelos, revisa las definiciones de `ForeignKey` y asegúrate de que los datos relacionados existen en la base de datos.

Con esta unidad, hemos cubierto en detalle el modelo de Categoría y los modelos relacionados en Django, proporcionando una base sólida para organizar y gestionar productos en un sistema de carrito de compras. Asegúrate de seguir las mejores prácticas y consideraciones de diseño para crear una aplicación robusta y escalable.

# Unidad: Vistas basadas en clases para operaciones CRUD

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en el desarrollo de vistas basadas en clases (Class-Based Views, CBVs) para implementar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en un sistema de carrito de compras utilizando Django. Las vistas basadas en clases proporcionan una manera más estructurada y reutilizable de manejar la lógica de las vistas en comparación con las vistas basadas en funciones (Function-Based Views, FBVs).

Al finalizar esta unidad, los estudiantes serán capaces de:
1. Comprender los conceptos fundamentales de las vistas basadas en clases en Django.
2. Implementar operaciones CRUD utilizando vistas basadas en clases.
3. Utilizar mixins y genéricos para simplificar la implementación de vistas.
4. Aplicar mejores prácticas en el diseño y desarrollo de vistas basadas en clases.
5. Realizar pruebas unitarias para asegurar la funcionalidad de las vistas.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

El objetivo es implementar un conjunto de vistas basadas en clases para manejar las operaciones CRUD de los productos en un sistema de carrito de compras. Estas vistas permitirán a los usuarios realizar las siguientes acciones:
- Crear un nuevo producto.
- Leer (listar y ver detalles) productos existentes.
- Actualizar la información de un producto.
- Eliminar un producto.

### Casos de uso

#### Caso de uso 1: Crear un nuevo producto
- **Actor**: Administrador
- **Descripción**: El administrador puede agregar un nuevo producto al sistema proporcionando detalles como nombre, descripción, precio y cantidad.
- **Precondiciones**: El administrador debe estar autenticado.
- **Postcondiciones**: El nuevo producto se guarda en la base de datos y se muestra en la lista de productos.

#### Caso de uso 2: Leer productos existentes
- **Actor**: Usuario
- **Descripción**: Los usuarios pueden ver una lista de productos disponibles y detalles individuales de cada producto.
- **Precondiciones**: Ninguna.
- **Postcondiciones**: La lista de productos se muestra al usuario, y los detalles de un producto específico se pueden ver al seleccionar un producto de la lista.

#### Caso de uso 3: Actualizar la información de un producto
- **Actor**: Administrador
- **Descripción**: El administrador puede actualizar la información de un producto existente.
- **Precondiciones**: El administrador debe estar autenticado.
- **Postcondiciones**: La información del producto se actualiza en la base de datos y se refleja en la lista de productos.

#### Caso de uso 4: Eliminar un producto
- **Actor**: Administrador
- **Descripción**: El administrador puede eliminar un producto del sistema.
- **Precondiciones**: El administrador debe estar autenticado.
- **Postcondiciones**: El producto se elimina de la base de datos y ya no se muestra en la lista de productos.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Autenticación]
    B -->|Administrador| C[Crear Producto]
    B -->|Administrador| D[Actualizar Producto]
    B -->|Administrador| E[Eliminar Producto]
    B -->|Usuario| F[Listar Productos]
    C --> G[Guardar en BD]
    D --> H[Actualizar en BD]
    E --> I[Eliminar de BD]
    F --> J[Mostrar Lista]
    G --> K[Mostrar Lista]
    H --> K
    I --> K
    K --> L[Fin]
```

### Requisitos no funcionales

1. **Seguridad**: Solo los administradores autenticados pueden crear, actualizar o eliminar productos.
2. **Rendimiento**: Las operaciones CRUD deben ser eficientes y no afectar el rendimiento general del sistema.
3. **Escalabilidad**: El sistema debe ser capaz de manejar un gran número de productos sin degradar el rendimiento.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar.
5. **Mantenibilidad**: El código debe ser modular y fácil de mantener.

## Implementación en Python

### Explicación paso a paso del código

Para implementar las operaciones CRUD utilizando vistas basadas en clases en Django, seguiremos estos pasos:

1. **Definir el modelo de Producto**: Este modelo representará los productos en nuestro sistema.
2. **Crear formularios para el modelo de Producto**: Utilizaremos formularios de Django para facilitar la creación y actualización de productos.
3. **Implementar vistas basadas en clases**: Utilizaremos vistas genéricas de Django para manejar las operaciones CRUD.
4. **Configurar URLs**: Definiremos las rutas necesarias para acceder a nuestras vistas.
5. **Crear templates**: Diseñaremos las plantillas HTML para mostrar y gestionar los productos.

### Código fuente completo y comentado

#### 1. Definir el modelo de Producto

```python
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    cantidad = models.PositiveIntegerField()

    def __str__(self):
        return self.nombre
```

#### 2. Crear formularios para el modelo de Producto

```python
from django import forms
from .models import Producto

class ProductoForm(forms.ModelForm):
    class Meta:
        model = Producto
        fields = ['nombre', 'descripcion', 'precio', 'cantidad']
```

#### 3. Implementar vistas basadas en clases

```python
from django.urls import reverse_lazy
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from .models import Producto
from .forms import ProductoForm

class ProductoListView(ListView):
    model = Producto
    template_name = 'productos/producto_list.html'

class ProductoDetailView(DetailView):
    model = Producto
    template_name = 'productos/producto_detail.html'

class ProductoCreateView(CreateView):
    model = Producto
    form_class = ProductoForm
    template_name = 'productos/producto_form.html'
    success_url = reverse_lazy('producto_list')

class ProductoUpdateView(UpdateView):
    model = Producto
    form_class = ProductoForm
    template_name = 'productos/producto_form.html'
    success_url = reverse_lazy('producto_list')

class ProductoDeleteView(DeleteView):
    model = Producto
    template_name = 'productos/producto_confirm_delete.html'
    success_url = reverse_lazy('producto_list')
```

#### 4. Configurar URLs

```python
from django.urls import path
from .views import ProductoListView, ProductoDetailView, ProductoCreateView, ProductoUpdateView, ProductoDeleteView

urlpatterns = [
    path('', ProductoListView.as_view(), name='producto_list'),
    path('<int:pk>/', ProductoDetailView.as_view(), name='producto_detail'),
    path('nuevo/', ProductoCreateView.as_view(), name='producto_create'),
    path('<int:pk>/editar/', ProductoUpdateView.as_view(), name='producto_update'),
    path('<int:pk>/eliminar/', ProductoDeleteView.as_view(), name='producto_delete'),
]
```

#### 5. Crear templates

**producto_list.html**
```html
{% extends "base_generic.html" %}

{% block content %}
  <h1>Lista de Productos</h1>
  <ul>
    {% for producto in object_list %}
      <li>
        <a href="{% url 'producto_detail' producto.pk %}">{{ producto.nombre }}</a>
      </li>
    {% endfor %}
  </ul>
  <a href="{% url 'producto_create' %}">Agregar nuevo producto</a>
{% endblock %}
```

**producto_detail.html**
```html
{% extends "base_generic.html" %}

{% block content %}
  <h1>{{ object.nombre }}</h1>
  <p>{{ object.descripcion }}</p>
  <p>Precio: {{ object.precio }}</p>
  <p>Cantidad: {{ object.cantidad }}</p>
  <a href="{% url 'producto_update' object.pk %}">Editar</a>
  <a href="{% url 'producto_delete' object.pk %}">Eliminar</a>
{% endblock %}
```

**producto_form.html**
```html
{% extends "base_generic.html" %}

{% block content %}
  <h1>{% if object %}Editar Producto{% else %}Agregar Producto{% endif %}</h1>
  <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Guardar</button>
  </form>
{% endblock %}
```

**producto_confirm_delete.html**
```html
{% extends "base_generic.html" %}

{% block content %}
  <h1>Eliminar Producto</h1>
  <p>¿Estás seguro de que deseas eliminar "{{ object.nombre }}"?</p>
  <form method="post">
    {% csrf_token %}
    <button type="submit">Eliminar</button>
  </form>
  <a href="{% url 'producto_list' %}">Cancelar</a>
{% endblock %}
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplos de uso

Para probar la funcionalidad de las vistas basadas en clases, podemos seguir estos pasos:

1. **Crear un nuevo producto**: Navegar a la URL `/nuevo/`, completar el formulario y enviar.
2. **Listar productos**: Navegar a la URL `/` para ver la lista de productos.
3. **Ver detalles de un producto**: Hacer clic en el nombre de un producto en la lista para ver sus detalles.
4. **Actualizar un producto**: Hacer clic en "Editar" en la página de detalles de un producto, completar el formulario y enviar.
5. **Eliminar un producto**: Hacer clic en "Eliminar" en la página de detalles de un producto y confirmar la eliminación.

#### Pruebas unitarias

```python
from django.test import TestCase
from django.urls import reverse
from .models import Producto

class ProductoTests(TestCase):

    def setUp(self):
        self.producto = Producto.objects.create(
            nombre='Producto de prueba',
            descripcion='Descripción del producto de prueba',
            precio=100.00,
            cantidad=10
        )

    def test_producto_list_view(self):
        response = self.client.get(reverse('producto_list'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, self.producto.nombre)
        self.assertTemplateUsed(response, 'productos/producto_list.html')

    def test_producto_detail_view(self):
        response = self.client.get(reverse('producto_detail', args=[self.producto.id]))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, self.producto.nombre)
        self.assertTemplateUsed(response, 'productos/producto_detail.html')

    def test_producto_create_view(self):
        response = self.client.post(reverse('producto_create'), {
            'nombre': 'Nuevo Producto',
            'descripcion': 'Descripción del nuevo producto',
            'precio': 200.00,
            'cantidad': 20
        })
        self.assertEqual(response.status_code, 302)
        self.assertEqual(Producto.objects.last().nombre, 'Nuevo Producto')

    def test_producto_update_view(self):
        response = self.client.post(reverse('producto_update', args=[self.producto.id]), {
            'nombre': 'Producto Actualizado',
            'descripcion': 'Descripción actualizada',
            'precio': 150.00,
            'cantidad': 15
        })
        self.assertEqual(response.status_code, 302)
        self.producto.refresh_from_db()
        self.assertEqual(self.producto.nombre, 'Producto Actualizado')

    def test_producto_delete_view(self):
        response = self.client.post(reverse('producto_delete', args=[self.producto.id]))
        self.assertEqual(response.status_code, 302)
        self.assertFalse(Producto.objects.filter(id=self.producto.id).exists())
```

### Mejores prácticas y consideraciones de diseño

1. **Reutilización de código**: Utilizar vistas genéricas y mixins para evitar la duplicación de código y facilitar el mantenimiento.
2. **Seguridad**: Asegurarse de que solo los usuarios autenticados y autorizados puedan realizar operaciones sensibles como crear, actualizar o eliminar productos.
3. **Validación de datos**: Utilizar formularios de Django para validar los datos de entrada y manejar errores de manera adecuada.
4. **Modularidad**: Mantener el código modular y organizado, separando las vistas, modelos, formularios y templates en archivos y directorios adecuados.
5. **Pruebas**: Escribir pruebas unitarias para verificar la funcionalidad de las vistas y asegurar que los cambios futuros no introduzcan errores.
6. **Escalabilidad**: Diseñar el sistema de manera que pueda escalar fácilmente, utilizando técnicas como la paginación para manejar grandes volúmenes de datos.
7. **Rendimiento**: Optimizar las consultas a la base de datos y utilizar técnicas de caché cuando sea necesario para mejorar el rendimiento.

En conclusión, las vistas basadas en clases en Django proporcionan una manera eficiente y estructurada de manejar las operaciones CRUD en un sistema de carrito de compras. Siguiendo las mejores prácticas y consideraciones de diseño, podemos crear un sistema robusto, seguro y fácil de mantener.

# Unidad: Templates con Django Template Language

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, exploraremos en profundidad el uso de Django Template Language (DTL) para la creación de templates en un sistema de carrito de compras. Los templates son una parte fundamental de cualquier aplicación web, ya que permiten la separación de la lógica de negocio y la presentación. Al finalizar esta unidad, serás capaz de:

1. Comprender la estructura y sintaxis básica de Django Template Language.
2. Implementar templates dinámicos que interactúan con vistas y modelos.
3. Utilizar filtros y tags de DTL para manipular datos en los templates.
4. Crear layouts reutilizables mediante el uso de herencia de templates.
5. Aplicar buenas prácticas en el diseño y organización de templates.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El objetivo de esta sección es proporcionar una guía detallada sobre cómo utilizar Django Template Language para crear y gestionar templates en un sistema de carrito de compras. Los templates deben permitir la visualización de productos, la gestión del carrito de compras y la finalización de la compra. Además, deben ser dinámicos y capaces de interactuar con las vistas y modelos de Django.

### Casos de Uso

1. **Visualización de Productos**:
   - **Actor**: Usuario.
   - **Descripción**: El usuario puede ver una lista de productos disponibles en la tienda.
   - **Precondiciones**: El usuario ha accedido a la página principal de la tienda.
   - **Postcondiciones**: El usuario ve una lista de productos con sus detalles.

2. **Gestión del Carrito de Compras**:
   - **Actor**: Usuario.
   - **Descripción**: El usuario puede añadir productos al carrito, ver los productos en el carrito y eliminar productos del carrito.
   - **Precondiciones**: El usuario ha seleccionado productos para añadir al carrito.
   - **Postcondiciones**: El usuario ve los productos en el carrito y puede modificar la cantidad o eliminar productos.

3. **Finalización de la Compra**:
   - **Actor**: Usuario.
   - **Descripción**: El usuario puede proceder a la compra de los productos en el carrito.
   - **Precondiciones**: El usuario ha revisado los productos en el carrito y desea finalizar la compra.
   - **Postcondiciones**: El usuario completa el proceso de compra y recibe una confirmación.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Visualización de Productos]
    B --> C[Gestión del Carrito de Compras]
    C --> D[Finalización de la Compra]
    D --> E[Confirmación de Compra]
```

### Requisitos No Funcionales

1. **Rendimiento**: Los templates deben ser eficientes y no causar demoras perceptibles en la carga de las páginas.
2. **Escalabilidad**: La estructura de los templates debe permitir la fácil adición de nuevas funcionalidades y productos.
3. **Seguridad**: Los templates deben estar diseñados para evitar vulnerabilidades como la inyección de código.
4. **Usabilidad**: La interfaz debe ser intuitiva y fácil de usar para los usuarios.

## Implementación en Python

### Explicación Paso a Paso del Código

Para implementar los templates utilizando Django Template Language, seguiremos los siguientes pasos:

1. **Configuración del Proyecto**: Asegúrate de tener un proyecto Django configurado y funcionando.
2. **Creación de Templates**: Crea templates para las diferentes vistas de la aplicación.
3. **Uso de Filtros y Tags**: Utiliza filtros y tags de DTL para manipular y mostrar datos dinámicamente.
4. **Herencia de Templates**: Implementa la herencia de templates para crear layouts reutilizables.
5. **Integración con Vistas**: Conecta los templates con las vistas para mostrar datos dinámicos.

### Código Fuente Completo y Comentado

#### 1. Configuración del Proyecto

Asegúrate de que tu proyecto Django esté configurado correctamente y que la aplicación `shop` esté incluida en `INSTALLED_APPS` en `settings.py`.

#### 2. Creación de Templates

Crea una carpeta `templates` dentro de tu aplicación `shop` y añade los siguientes templates:

**base.html**:
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Tienda Online{% endblock %}</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}">
</head>
<body>
    <header>
        <h1>Tienda Online</h1>
        <nav>
            <ul>
                <li><a href="{% url 'product_list' %}">Productos</a></li>
                <li><a href="{% url 'cart_detail' %}">Carrito</a></li>
            </ul>
        </nav>
    </header>
    <main>
        {% block content %}{% endblock %}
    </main>
    <footer>
        <p>&copy; 2023 Tienda Online</p>
    </footer>
</body>
</html>
```

**product_list.html**:
```html
{% extends "base.html" %}

{% block title %}Lista de Productos{% endblock %}

{% block content %}
<h2>Lista de Productos</h2>
<ul>
    {% for product in products %}
    <li>
        <h3>{{ product.name }}</h3>
        <p>{{ product.description }}</p>
        <p>Precio: ${{ product.price }}</p>
        <form method="post" action="{% url 'add_to_cart' product.id %}">
            {% csrf_token %}
            <button type="submit">Añadir al Carrito</button>
        </form>
    </li>
    {% endfor %}
</ul>
{% endblock %}
```

**cart_detail.html**:
```html
{% extends "base.html" %}

{% block title %}Detalle del Carrito{% endblock %}

{% block content %}
<h2>Detalle del Carrito</h2>
<ul>
    {% for item in cart.items %}
    <li>
        <h3>{{ item.product.name }}</h3>
        <p>Cantidad: {{ item.quantity }}</p>
        <p>Precio: ${{ item.total_price }}</p>
        <form method="post" action="{% url 'remove_from_cart' item.id %}">
            {% csrf_token %}
            <button type="submit">Eliminar</button>
        </form>
    </li>
    {% endfor %}
</ul>
<p>Total: ${{ cart.total_price }}</p>
<form method="post" action="{% url 'checkout' %}">
    {% csrf_token %}
    <button type="submit">Finalizar Compra</button>
</form>
{% endblock %}
```

#### 3. Uso de Filtros y Tags

En los templates anteriores, hemos utilizado varios filtros y tags de DTL:

- **{% extends %}**: Para heredar de otro template.
- **{% block %}**: Para definir bloques de contenido que pueden ser sobrescritos en templates hijos.
- **{% for %}**: Para iterar sobre una lista de objetos.
- **{{ variable }}**: Para mostrar el valor de una variable.
- **{% csrf_token %}**: Para incluir un token CSRF en formularios.
- **{% url %}**: Para generar URLs dinámicamente.

#### 4. Herencia de Templates

La herencia de templates permite crear un layout base (`base.html`) que puede ser reutilizado en otros templates (`product_list.html`, `cart_detail.html`). Esto facilita la consistencia y el mantenimiento del código.

#### 5. Integración con Vistas

Asegúrate de que las vistas correspondientes estén configuradas para renderizar los templates:

**views.py**:
```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Product, Cart

def product_list(request):
    products = Product.objects.all()
    return render(request, 'product_list.html', {'products': products})

def cart_detail(request):
    cart = Cart.objects.get(user=request.user)
    return render(request, 'cart_detail.html', {'cart': cart})

def add_to_cart(request, product_id):
    product = get_object_or_404(Product, id=product_id)
    cart = Cart.objects.get(user=request.user)
    cart.add_product(product)
    return redirect('cart_detail')

def remove_from_cart(request, item_id):
    cart = Cart.objects.get(user=request.user)
    cart.remove_item(item_id)
    return redirect('cart_detail')

def checkout(request):
    cart = Cart.objects.get(user=request.user)
    cart.checkout()
    return redirect('confirmation')
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplos de Uso

Para ver la lista de productos, el usuario puede navegar a la URL correspondiente (`/products/`). Para gestionar el carrito de compras, el usuario puede añadir productos desde la lista de productos y ver el detalle del carrito en la URL correspondiente (`/cart/`).

#### Pruebas Unitarias

**tests.py**:
```python
from django.test import TestCase
from django.urls import reverse
from .models import Product, Cart

class CartTests(TestCase):
    def setUp(self):
        self.product = Product.objects.create(name='Producto 1', description='Descripción del producto 1', price=100)
        self.cart = Cart.objects.create(user=self.user)

    def test_add_to_cart(self):
        response = self.client.post(reverse('add_to_cart', args=[self.product.id]))
        self.assertEqual(response.status_code, 302)
        self.assertEqual(self.cart.items.count(), 1)

    def test_remove_from_cart(self):
        self.cart.add_product(self.product)
        item = self.cart.items.first()
        response = self.client.post(reverse('remove_from_cart', args=[item.id]))
        self.assertEqual(response.status_code, 302)
        self.assertEqual(self.cart.items.count(), 0)

    def test_checkout(self):
        self.cart.add_product(self.product)
        response = self.client.post(reverse('checkout'))
        self.assertEqual(response.status_code, 302)
        self.assertEqual(self.cart.items.count(), 0)
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Separación de Concerns**: Mantén la lógica de negocio en las vistas y modelos, y la lógica de presentación en los templates.
2. **Reutilización de Código**: Utiliza la herencia de templates para crear layouts reutilizables y consistentes.
3. **Seguridad**: Siempre utiliza `{% csrf_token %}` en formularios para proteger contra ataques CSRF.
4. **Rendimiento**: Minimiza el uso de consultas a la base de datos en los templates. Prefiere realizar las consultas en las vistas y pasar los datos necesarios a los templates.
5. **Legibilidad**: Mantén los templates limpios y bien organizados. Utiliza comentarios y nombres descriptivos para las variables y bloques.

En resumen, Django Template Language es una herramienta poderosa para crear templates dinámicos y reutilizables en una aplicación web. Siguiendo las mejores prácticas y utilizando las funcionalidades de DTL, puedes crear una interfaz de usuario eficiente y mantenible para tu sistema de carrito de compras.

# Unidad: Uso de Formularios de Django

## 1. Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, profundizaremos en el uso de formularios en Django, un aspecto crucial para la creación de aplicaciones web dinámicas y funcionales. Los formularios permiten a los usuarios interactuar con la aplicación, enviar datos y realizar diversas operaciones, como agregar productos al carrito de compras, registrarse, iniciar sesión, y más.

### Objetivos de Aprendizaje

Al finalizar esta unidad, serás capaz de:

1. Comprender la estructura y funcionamiento de los formularios en Django.
2. Crear formularios personalizados utilizando `forms.Form` y `forms.ModelForm`.
3. Validar datos de formularios y manejar errores.
4. Integrar formularios en vistas y templates.
5. Implementar formularios avanzados con widgets y validaciones personalizadas.
6. Aplicar mejores prácticas en el diseño y uso de formularios en Django.

## 2. Documento Funcional de Requerimientos

### a. Descripción Detallada de la Funcionalidad

Los formularios en Django son una herramienta poderosa que permite la interacción entre el usuario y la aplicación. En el contexto de un sistema de carrito de compras, los formularios pueden ser utilizados para diversas funcionalidades, tales como:

- Registro de usuarios.
- Inicio de sesión.
- Agregar productos al carrito.
- Actualizar la cantidad de productos en el carrito.
- Realizar el checkout y procesar pagos.

### b. Casos de Uso

#### Caso de Uso 1: Registro de Usuarios

**Descripción**: Permitir a los nuevos usuarios registrarse en la plataforma proporcionando su información personal.

**Actores**: Usuario

**Flujo Principal**:
1. El usuario accede a la página de registro.
2. El usuario completa el formulario con su información personal (nombre, correo electrónico, contraseña).
3. El usuario envía el formulario.
4. El sistema valida los datos y crea una nueva cuenta de usuario.
5. El sistema redirige al usuario a la página de inicio de sesión.

#### Caso de Uso 2: Agregar Productos al Carrito

**Descripción**: Permitir a los usuarios agregar productos al carrito de compras.

**Actores**: Usuario

**Flujo Principal**:
1. El usuario navega por el catálogo de productos.
2. El usuario selecciona un producto y accede a su página de detalles.
3. El usuario completa el formulario para especificar la cantidad y otras opciones del producto.
4. El usuario envía el formulario.
5. El sistema valida los datos y agrega el producto al carrito de compras.
6. El sistema muestra un mensaje de confirmación y actualiza el carrito.

### c. Diagramas de Flujo

#### Diagrama de Flujo para el Registro de Usuarios

```mermaid
graph TD;
    A[Inicio] --> B[Acceder a la página de registro]
    B --> C[Completar formulario de registro]
    C --> D[Enviar formulario]
    D --> E{Validar datos}
    E -->|Datos válidos| F[Crear cuenta de usuario]
    F --> G[Redirigir a la página de inicio de sesión]
    E -->|Datos inválidos| H[Mostrar errores]
    H --> C
```

#### Diagrama de Flujo para Agregar Productos al Carrito

```mermaid
graph TD;
    A[Inicio] --> B[Navegar por el catálogo de productos]
    B --> C[Seleccionar producto]
    C --> D[Acceder a la página de detalles del producto]
    D --> E[Completar formulario de producto]
    E --> F[Enviar formulario]
    F --> G{Validar datos}
    G -->|Datos válidos| H[Agregar producto al carrito]
    H --> I[Mostrar mensaje de confirmación]
    G -->|Datos inválidos| J[Mostrar errores]
    J --> E
```

### d. Requisitos No Funcionales

1. **Usabilidad**: Los formularios deben ser intuitivos y fáciles de usar, con mensajes de error claros y útiles.
2. **Seguridad**: Los datos enviados a través de los formularios deben ser validados y sanitizados para evitar ataques como la inyección SQL y el cross-site scripting (XSS).
3. **Rendimiento**: La validación de formularios debe ser eficiente para no afectar la experiencia del usuario.
4. **Escalabilidad**: El sistema debe ser capaz de manejar un gran número de formularios enviados simultáneamente.

## 3. Implementación en Python

### a. Explicación Paso a Paso del Código

Para implementar los formularios en Django, seguiremos los siguientes pasos:

1. **Crear Formularios**: Utilizaremos `forms.Form` y `forms.ModelForm` para crear formularios personalizados.
2. **Validar Datos**: Implementaremos validaciones personalizadas para asegurar que los datos ingresados sean correctos.
3. **Integrar Formularios en Vistas**: Utilizaremos vistas basadas en clases para manejar la lógica de los formularios.
4. **Renderizar Formularios en Templates**: Utilizaremos Django Template Language (DTL) para mostrar los formularios en las páginas web.

### b. Código Fuente Completo y Comentado

#### Formulario de Registro de Usuarios

**forms.py**
```python
from django import forms
from django.contrib.auth.models import User
from django.core.exceptions import ValidationError

class RegistroUsuarioForm(forms.ModelForm):
    password = forms.CharField(widget=forms.PasswordInput)
    password_confirm = forms.CharField(widget=forms.PasswordInput, label="Confirmar contraseña")

    class Meta:
        model = User
        fields = ['username', 'email', 'password']

    def clean_password_confirm(self):
        password = self.cleaned_data.get('password')
        password_confirm = self.cleaned_data.get('password_confirm')
        if password and password_confirm and password != password_confirm:
            raise ValidationError("Las contraseñas no coinciden")
        return password_confirm
```

**views.py**
```python
from django.shortcuts import render, redirect
from django.contrib.auth import login, authenticate
from .forms import RegistroUsuarioForm

def registro(request):
    if request.method == 'POST':
        form = RegistroUsuarioForm(request.POST)
        if form.is_valid():
            user = form.save(commit=False)
            user.set_password(form.cleaned_data['password'])
            user.save()
            login(request, user)
            return redirect('inicio')
    else:
        form = RegistroUsuarioForm()
    return render(request, 'registro.html', {'form': form})
```

**registro.html**
```html
{% extends "base.html" %}

{% block content %}
  <h2>Registro de Usuario</h2>
  <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Registrarse</button>
  </form>
{% endblock %}
```

#### Formulario para Agregar Productos al Carrito

**forms.py**
```python
from django import forms
from .models import Producto

class AgregarProductoForm(forms.Form):
    cantidad = forms.IntegerField(min_value=1, label="Cantidad")

    def __init__(self, *args, **kwargs):
        self.producto = kwargs.pop('producto')
        super().__init__(*args, **kwargs)

    def clean_cantidad(self):
        cantidad = self.cleaned_data.get('cantidad')
        if cantidad > self.producto.stock:
            raise forms.ValidationError("No hay suficiente stock disponible")
        return cantidad
```

**views.py**
```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Producto
from .forms import AgregarProductoForm

def agregar_al_carrito(request, producto_id):
    producto = get_object_or_404(Producto, id=producto_id)
    if request.method == 'POST':
        form = AgregarProductoForm(request.POST, producto=producto)
        if form.is_valid():
            # Lógica para agregar el producto al carrito
            return redirect('carrito')
    else:
        form = AgregarProductoForm(producto=producto)
    return render(request, 'agregar_al_carrito.html', {'form': form, 'producto': producto})
```

**agregar_al_carrito.html**
```html
{% extends "base.html" %}

{% block content %}
  <h2>Agregar {{ producto.nombre }} al Carrito</h2>
  <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Agregar al Carrito</button>
  </form>
{% endblock %}
```

### c. Ejemplos de Uso y Pruebas Unitarias

**Ejemplo de Uso: Registro de Usuarios**

Para probar el formulario de registro de usuarios, sigue estos pasos:

1. Accede a la URL `/registro/`.
2. Completa el formulario con un nombre de usuario, correo electrónico y contraseña.
3. Envía el formulario.
4. Verifica que el usuario ha sido creado y redirigido a la página de inicio de sesión.

**Pruebas Unitarias**

**tests.py**
```python
from django.test import TestCase
from django.urls import reverse
from django.contrib.auth.models import User

class RegistroUsuarioTest(TestCase):
    def test_registro_usuario(self):
        response = self.client.post(reverse('registro'), {
            'username': 'testuser',
            'email': 'testuser@example.com',
            'password': 'password123',
            'password_confirm': 'password123'
        })
        self.assertEqual(response.status_code, 302)
        self.assertTrue(User.objects.filter(username='testuser').exists())
```

**Ejemplo de Uso: Agregar Productos al Carrito**

Para probar el formulario de agregar productos al carrito, sigue estos pasos:

1. Accede a la página de detalles de un producto.
2. Completa el formulario con la cantidad deseada.
3. Envía el formulario.
4. Verifica que el producto ha sido agregado al carrito y se muestra un mensaje de confirmación.

**Pruebas Unitarias**

**tests.py**
```python
from django.test import TestCase
from django.urls import reverse
from .models import Producto

class AgregarProductoCarritoTest(TestCase):
    def setUp(self):
        self.producto = Producto.objects.create(nombre='Producto Test', stock=10, precio=100)

    def test_agregar_producto_carrito(self):
        response = self.client.post(reverse('agregar_al_carrito', args=[self.producto.id]), {
            'cantidad': 2
        })
        self.assertEqual(response.status_code, 302)
        # Verificar lógica de agregar al carrito
```

## 4. Mejores Prácticas y Consideraciones de Diseño

### Mejores Prácticas

1. **Validación de Datos**: Siempre valida los datos de los formularios tanto en el lado del cliente como en el servidor para asegurar la integridad y seguridad de los datos.
2. **Mensajes de Error**: Proporciona mensajes de error claros y específicos para ayudar a los usuarios a corregir sus entradas.
3. **Uso de Widgets**: Utiliza widgets personalizados para mejorar la experiencia del usuario y hacer los formularios más intuitivos.
4. **Reutilización de Formularios**: Define formularios reutilizables para evitar la duplicación de código y facilitar el mantenimiento.

### Consideraciones de Diseño

1. **Seguridad**: Asegúrate de proteger los formularios contra ataques comunes como CSRF (Cross-Site Request Forgery) utilizando el token CSRF de Django.
2. **Rendimiento**: Optimiza la validación de formularios para minimizar el impacto en el rendimiento de la aplicación.
3. **Escalabilidad**: Diseña los formularios y la lógica de validación para manejar un gran número de solicitudes simultáneas sin degradar el rendimiento.
4. **Accesibilidad**: Asegúrate de que los formularios sean accesibles para todos los usuarios, incluyendo aquellos con discapacidades.

### Consejos para la Depuración y Solución de Problemas Comunes

1. **Mensajes de Error**: Revisa los mensajes de error proporcionados por Django para identificar problemas específicos con los formularios.
2. **Pruebas Unitarias**: Implementa pruebas unitarias para verificar la funcionalidad y validación de los formularios.
3. **Depuración en el Navegador**: Utiliza herramientas de desarrollo del navegador para inspeccionar y depurar formularios en el lado del cliente.
4. **Logs del Servidor**: Revisa los logs del servidor para identificar y solucionar problemas con la lógica de los formularios en el lado del servidor.

Con estos conocimientos y prácticas, estarás bien equipado para implementar formularios efectivos y seguros en tu aplicación Django, mejorando la interacción del usuario y la funcionalidad del sistema de carrito de compras.

# Unidad: Implementación de Carritos de Compras con Sesiones

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en la implementación de carritos de compras utilizando sesiones en Django. Este tema es crucial para cualquier aplicación de comercio electrónico, ya que permite a los usuarios agregar, modificar y eliminar productos de su carrito de compras de manera eficiente y persistente durante su sesión de navegación.

### Objetivos de Aprendizaje

1. Comprender el concepto de sesiones en Django y cómo se utilizan para mantener el estado del carrito de compras.
2. Implementar vistas basadas en clases para gestionar las operaciones del carrito de compras.
3. Utilizar templates de Django para mostrar el contenido del carrito de compras al usuario.
4. Asegurar que el carrito de compras sea seguro, eficiente y escalable.
5. Realizar pruebas unitarias para garantizar la funcionalidad correcta del carrito de compras.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

El carrito de compras es una funcionalidad esencial en cualquier aplicación de comercio electrónico. Permite a los usuarios seleccionar productos, ver un resumen de los productos seleccionados, modificar las cantidades y proceder al pago. Utilizaremos sesiones para almacenar el estado del carrito de compras, lo que permite que los datos persistan mientras el usuario navega por el sitio web.

#### Funcionalidades Principales

1. **Agregar productos al carrito**: Los usuarios pueden agregar productos al carrito desde la página de detalles del producto.
2. **Ver el contenido del carrito**: Los usuarios pueden ver un resumen de los productos en su carrito, incluyendo el nombre del producto, cantidad, precio y el total.
3. **Modificar cantidades**: Los usuarios pueden aumentar o disminuir la cantidad de cada producto en el carrito.
4. **Eliminar productos del carrito**: Los usuarios pueden eliminar productos del carrito.
5. **Vaciar el carrito**: Los usuarios pueden vaciar todo el contenido del carrito de una sola vez.

### Casos de Uso

#### Caso de Uso 1: Agregar Producto al Carrito

**Actor**: Usuario

**Descripción**: El usuario selecciona un producto y lo agrega al carrito de compras.

**Flujo Principal**:
1. El usuario navega a la página de detalles del producto.
2. El usuario selecciona la cantidad deseada.
3. El usuario hace clic en el botón "Agregar al carrito".
4. El sistema agrega el producto al carrito y muestra un mensaje de confirmación.

#### Caso de Uso 2: Ver Contenido del Carrito

**Actor**: Usuario

**Descripción**: El usuario visualiza el contenido actual de su carrito de compras.

**Flujo Principal**:
1. El usuario hace clic en el enlace "Ver carrito".
2. El sistema muestra una lista de productos en el carrito, incluyendo nombre, cantidad, precio y total.

#### Caso de Uso 3: Modificar Cantidades

**Actor**: Usuario

**Descripción**: El usuario modifica la cantidad de un producto en el carrito.

**Flujo Principal**:
1. El usuario navega a la página del carrito.
2. El usuario cambia la cantidad de un producto.
3. El sistema actualiza el carrito y muestra el nuevo total.

#### Caso de Uso 4: Eliminar Producto del Carrito

**Actor**: Usuario

**Descripción**: El usuario elimina un producto del carrito.

**Flujo Principal**:
1. El usuario navega a la página del carrito.
2. El usuario hace clic en el botón "Eliminar" junto al producto.
3. El sistema elimina el producto del carrito y muestra el nuevo total.

#### Caso de Uso 5: Vaciar el Carrito

**Actor**: Usuario

**Descripción**: El usuario vacía todo el contenido del carrito de compras.

**Flujo Principal**:
1. El usuario navega a la página del carrito.
2. El usuario hace clic en el botón "Vaciar carrito".
3. El sistema elimina todos los productos del carrito y muestra un mensaje de confirmación.

### Diagramas de Flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Navegar a la página de detalles del producto]
    B --> C[Seleccionar cantidad]
    C --> D[Hacer clic en "Agregar al carrito"]
    D --> E[Producto agregado al carrito]
    E --> F[Mostrar mensaje de confirmación]

    G[Inicio] --> H[Hacer clic en "Ver carrito"]
    H --> I[Mostrar contenido del carrito]

    J[Inicio] --> K[Navegar a la página del carrito]
    K --> L[Modificar cantidad]
    L --> M[Actualizar carrito]
    M --> N[Mostrar nuevo total]

    O[Inicio] --> P[Navegar a la página del carrito]
    P --> Q[Hacer clic en "Eliminar"]
    Q --> R[Eliminar producto del carrito]
    R --> S[Mostrar nuevo total]

    T[Inicio] --> U[Navegar a la página del carrito]
    U --> V[Hacer clic en "Vaciar carrito"]
    V --> W[Eliminar todos los productos]
    W --> X[Mostrar mensaje de confirmación]
```

### Requisitos No Funcionales

1. **Seguridad**: El carrito de compras debe ser seguro, evitando ataques como la manipulación de datos en las sesiones.
2. **Rendimiento**: El sistema debe ser capaz de manejar múltiples usuarios simultáneamente sin degradar el rendimiento.
3. **Escalabilidad**: La implementación debe ser escalable para soportar un aumento en el número de usuarios y productos.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar.
5. **Compatibilidad**: El sistema debe ser compatible con los navegadores web más comunes.

## Implementación en Python

### Explicación Paso a Paso del Código

Para implementar el carrito de compras utilizando sesiones en Django, seguiremos los siguientes pasos:

1. **Configurar las URLs**: Definiremos las rutas necesarias para las operaciones del carrito de compras.
2. **Crear las Vistas**: Implementaremos vistas basadas en clases para manejar las operaciones del carrito.
3. **Definir los Templates**: Crearemos templates para mostrar el contenido del carrito al usuario.
4. **Gestionar las Sesiones**: Utilizaremos las sesiones de Django para almacenar y gestionar el estado del carrito.

### Código Fuente Completo y Comentado

#### Configuración de URLs

Primero, configuramos las URLs en `urls.py`:

```python
from django.urls import path
from .views import AddToCartView, CartDetailView, UpdateCartView, RemoveFromCartView, ClearCartView

urlpatterns = [
    path('cart/add/<int:product_id>/', AddToCartView.as_view(), name='add_to_cart'),
    path('cart/', CartDetailView.as_view(), name='cart_detail'),
    path('cart/update/<int:product_id>/', UpdateCartView.as_view(), name='update_cart'),
    path('cart/remove/<int:product_id>/', RemoveFromCartView.as_view(), name='remove_from_cart'),
    path('cart/clear/', ClearCartView.as_view(), name='clear_cart'),
]
```

#### Creación de Vistas

A continuación, implementamos las vistas en `views.py`:

```python
from django.shortcuts import redirect, render
from django.views import View
from django.contrib import messages
from .models import Product

class AddToCartView(View):
    def post(self, request, product_id):
        product = Product.objects.get(id=product_id)
        cart = request.session.get('cart', {})
        if product_id in cart:
            cart[product_id]['quantity'] += 1
        else:
            cart[product_id] = {'name': product.name, 'price': product.price, 'quantity': 1}
        request.session['cart'] = cart
        messages.success(request, 'Producto agregado al carrito.')
        return redirect('cart_detail')

class CartDetailView(View):
    def get(self, request):
        cart = request.session.get('cart', {})
        total = sum(item['price'] * item['quantity'] for item in cart.values())
        return render(request, 'cart_detail.html', {'cart': cart, 'total': total})

class UpdateCartView(View):
    def post(self, request, product_id):
        cart = request.session.get('cart', {})
        if product_id in cart:
            cart[product_id]['quantity'] = int(request.POST.get('quantity'))
            request.session['cart'] = cart
            messages.success(request, 'Cantidad actualizada.')
        return redirect('cart_detail')

class RemoveFromCartView(View):
    def post(self, request, product_id):
        cart = request.session.get('cart', {})
        if product_id in cart:
            del cart[product_id]
            request.session['cart'] = cart
            messages.success(request, 'Producto eliminado del carrito.')
        return redirect('cart_detail')

class ClearCartView(View):
    def post(self, request):
        request.session['cart'] = {}
        messages.success(request, 'Carrito vaciado.')
        return redirect('cart_detail')
```

#### Definición de Templates

Creamos los templates necesarios en `templates/cart_detail.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Carrito de Compras</title>
</head>
<body>
    <h1>Carrito de Compras</h1>
    <table>
        <thead>
            <tr>
                <th>Producto</th>
                <th>Cantidad</th>
                <th>Precio</th>
                <th>Total</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for product_id, item in cart.items %}
            <tr>
                <td>{{ item.name }}</td>
                <td>
                    <form method="post" action="{% url 'update_cart' product_id %}">
                        {% csrf_token %}
                        <input type="number" name="quantity" value="{{ item.quantity }}" min="1">
                        <button type="submit">Actualizar</button>
                    </form>
                </td>
                <td>{{ item.price }}</td>
                <td>{{ item.price|multiply:item.quantity }}</td>
                <td>
                    <form method="post" action="{% url 'remove_from_cart' product_id %}">
                        {% csrf_token %}
                        <button type="submit">Eliminar</button>
                    </form>
                </td>
            </tr>
            {% endfor %}
        </tbody>
        <tfoot>
            <tr>
                <td colspan="3">Total</td>
                <td>{{ total }}</td>
                <td>
                    <form method="post" action="{% url 'clear_cart' %}">
                        {% csrf_token %}
                        <button type="submit">Vaciar Carrito</button>
                    </form>
                </td>
            </tr>
        </tfoot>
    </table>
</body>
</html>
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplos de Uso

Para agregar un producto al carrito, el usuario debe navegar a la página de detalles del producto y hacer clic en "Agregar al carrito". Luego, puede ver el contenido del carrito haciendo clic en "Ver carrito".

#### Pruebas Unitarias

Implementamos pruebas unitarias en `tests.py`:

```python
from django.test import TestCase
from django.urls import reverse
from .models import Product

class CartTests(TestCase):
    def setUp(self):
        self.product = Product.objects.create(name='Producto 1', price=100)

    def test_add_to_cart(self):
        response = self.client.post(reverse('add_to_cart', args=[self.product.id]))
        self.assertEqual(response.status_code, 302)
        self.assertIn(str(self.product.id), self.client.session['cart'])

    def test_view_cart(self):
        self.client.post(reverse('add_to_cart', args=[self.product.id]))
        response = self.client.get(reverse('cart_detail'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Producto 1')

    def test_update_cart(self):
        self.client.post(reverse('add_to_cart', args=[self.product.id]))
        response = self.client.post(reverse('update_cart', args=[self.product.id]), {'quantity': 2})
        self.assertEqual(response.status_code, 302)
        self.assertEqual(self.client.session['cart'][str(self.product.id)]['quantity'], 2)

    def test_remove_from_cart(self):
        self.client.post(reverse('add_to_cart', args=[self.product.id]))
        response = self.client.post(reverse('remove_from_cart', args=[self.product.id]))
        self.assertEqual(response.status_code, 302)
        self.assertNotIn(str(self.product.id), self.client.session['cart'])

    def test_clear_cart(self):
        self.client.post(reverse('add_to_cart', args=[self.product.id]))
        response = self.client.post(reverse('clear_cart'))
        self.assertEqual(response.status_code, 302)
        self.assertEqual(self.client.session['cart'], {})
```

## Mejores Prácticas y Consideraciones de Diseño

### Seguridad

- **Protección CSRF**: Asegúrate de incluir tokens CSRF en todos los formularios para prevenir ataques CSRF.
- **Validación de Datos**: Valida todos los datos de entrada para evitar la manipulación de datos.

### Rendimiento

- **Optimización de Consultas**: Minimiza el número de consultas a la base de datos utilizando técnicas como la prefetch_related y select_related.
- **Uso de Caché**: Considera el uso de caché para almacenar datos frecuentemente accedidos.

### Escalabilidad

- **Desacoplamiento de Componentes**: Mantén el código modular y desacoplado para facilitar la escalabilidad.
- **Balanceo de Carga**: Utiliza técnicas de balanceo de carga para distribuir el tráfico entre múltiples servidores.

### Usabilidad

- **Interfaz Intuitiva**: Diseña una interfaz de usuario que sea fácil de entender y usar.
- **Feedback al Usuario**: Proporciona feedback claro y conciso al usuario después de cada acción.

### Compatibilidad

- **Pruebas en Múltiples Navegadores**: Asegúrate de que tu aplicación sea compatible con los navegadores más comunes.
- **Responsive Design**: Diseña la interfaz de usuario para que sea responsive y funcione bien en dispositivos móviles.

### Depuración y Solución de Problemas

- **Logging**: Utiliza el logging para registrar eventos importantes y errores.
- **Pruebas Unitarias**: Implementa pruebas unitarias para asegurar la funcionalidad correcta del carrito de compras.
- **Herramientas de Depuración**: Utiliza herramientas de depuración como el Django Debug Toolbar para identificar y solucionar problemas.

En conclusión, la implementación de un carrito de compras utilizando sesiones en Django es una tarea compleja pero manejable. Siguiendo las mejores prácticas y consideraciones de diseño, puedes crear una funcionalidad de carrito de compras que sea segura, eficiente y fácil de usar.

# Unidad: Integración de Django Crispy Forms para mejorar la UI

## 1. Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la integración de Django Crispy Forms para mejorar la interfaz de usuario (UI) de nuestro sistema de carrito de compras. Django Crispy Forms es una biblioteca que facilita la creación de formularios atractivos y funcionales, permitiendo a los desarrolladores personalizar la presentación de los formularios de Django sin necesidad de escribir HTML adicional. Al finalizar esta unidad, serás capaz de:

1. Comprender la importancia de una buena UI en formularios.
2. Integrar Django Crispy Forms en un proyecto Django.
3. Personalizar formularios utilizando Crispy Forms.
4. Implementar formularios mejorados en el contexto de un sistema de carrito de compras.

## 2. Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es mejorar la interfaz de usuario de los formularios en nuestro sistema de carrito de compras utilizando Django Crispy Forms. Esto incluye:

- Instalación y configuración de Django Crispy Forms en el proyecto.
- Modificación de formularios existentes para utilizar Crispy Forms.
- Personalización de la apariencia de los formularios mediante el uso de layouts y helpers de Crispy Forms.

### b. Casos de uso

#### Caso de uso 1: Instalación y configuración de Django Crispy Forms

**Actor:** Desarrollador

**Descripción:** El desarrollador instala y configura Django Crispy Forms en el proyecto Django.

**Precondiciones:** El proyecto Django está configurado y funcionando.

**Postcondiciones:** Django Crispy Forms está instalado y configurado correctamente.

#### Caso de uso 2: Modificación de formularios existentes

**Actor:** Desarrollador

**Descripción:** El desarrollador modifica los formularios existentes para utilizar Django Crispy Forms.

**Precondiciones:** Los formularios están definidos y funcionando en el proyecto.

**Postcondiciones:** Los formularios utilizan Django Crispy Forms y tienen una mejor apariencia.

#### Caso de uso 3: Personalización de formularios

**Actor:** Desarrollador

**Descripción:** El desarrollador personaliza la apariencia de los formularios utilizando layouts y helpers de Crispy Forms.

**Precondiciones:** Django Crispy Forms está integrado en los formularios.

**Postcondiciones:** Los formularios tienen una apariencia personalizada y mejorada.

### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Instalación de Django Crispy Forms]
    B --> C[Configuración de Django Crispy Forms]
    C --> D[Modificación de formularios existentes]
    D --> E[Personalización de formularios]
    E --> F[Fin]
```

### d. Requisitos no funcionales

1. **Usabilidad:** Los formularios deben ser intuitivos y fáciles de usar para los usuarios finales.
2. **Rendimiento:** La integración de Crispy Forms no debe afectar negativamente el rendimiento del sistema.
3. **Mantenibilidad:** El código debe ser fácil de mantener y extender.
4. **Compatibilidad:** Los formularios deben ser compatibles con los principales navegadores web.

## 3. Implementación en Python

### a. Explicación paso a paso del código

#### Paso 1: Instalación de Django Crispy Forms

Para instalar Django Crispy Forms, ejecuta el siguiente comando en tu terminal:

```bash
pip install django-crispy-forms
```

#### Paso 2: Configuración de Django Crispy Forms

Agrega `crispy_forms` a la lista de aplicaciones instaladas en `settings.py`:

```python
INSTALLED_APPS = [
    ...
    'crispy_forms',
]
```

Configura el paquete de CSS que deseas usar. Por ejemplo, para Bootstrap 4:

```python
CRISPY_TEMPLATE_PACK = 'bootstrap4'
```

#### Paso 3: Modificación de formularios existentes

Supongamos que tenemos un formulario de registro de usuario en `forms.py`:

```python
from django import forms

class UserRegistrationForm(forms.Form):
    username = forms.CharField(max_length=100)
    email = forms.EmailField()
    password = forms.CharField(widget=forms.PasswordInput)
```

Para utilizar Django Crispy Forms, modifica el formulario de la siguiente manera:

```python
from crispy_forms.helper import FormHelper
from crispy_forms.layout import Submit

class UserRegistrationForm(forms.Form):
    username = forms.CharField(max_length=100)
    email = forms.EmailField()
    password = forms.CharField(widget=forms.PasswordInput)

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.helper = FormHelper()
        self.helper.form_method = 'post'
        self.helper.add_input(Submit('submit', 'Register'))
```

#### Paso 4: Personalización de formularios

Puedes personalizar aún más el formulario utilizando layouts y helpers de Crispy Forms. Por ejemplo:

```python
from crispy_forms.layout import Layout, Fieldset, ButtonHolder, Submit

class UserRegistrationForm(forms.Form):
    username = forms.CharField(max_length=100)
    email = forms.EmailField()
    password = forms.CharField(widget=forms.PasswordInput)

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.helper = FormHelper()
        self.helper.form_method = 'post'
        self.helper.layout = Layout(
            Fieldset(
                'Register',
                'username',
                'email',
                'password',
            ),
            ButtonHolder(
                Submit('submit', 'Register')
            )
        )
```

### b. Código fuente completo y comentado

```python
# forms.py

from django import forms
from crispy_forms.helper import FormHelper
from crispy_forms.layout import Layout, Fieldset, ButtonHolder, Submit

class UserRegistrationForm(forms.Form):
    username = forms.CharField(max_length=100)
    email = forms.EmailField()
    password = forms.CharField(widget=forms.PasswordInput)

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        # Inicializa el FormHelper
        self.helper = FormHelper()
        self.helper.form_method = 'post'
        # Define el layout del formulario
        self.helper.layout = Layout(
            Fieldset(
                'Register',
                'username',
                'email',
                'password',
            ),
            ButtonHolder(
                Submit('submit', 'Register')
            )
        )
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Para renderizar el formulario en una vista, puedes hacer lo siguiente:

```python
# views.py

from django.shortcuts import render
from .forms import UserRegistrationForm

def register(request):
    if request.method == 'POST':
        form = UserRegistrationForm(request.POST)
        if form.is_valid():
            # Procesar el formulario
            pass
    else:
        form = UserRegistrationForm()

    return render(request, 'register.html', {'form': form})
```

En el template `register.html`, renderiza el formulario utilizando Crispy Forms:

```html
<!-- register.html -->

{% load crispy_forms_tags %}

<form method="post">
    {% csrf_token %}
    {{ form|crispy }}
</form>
```

#### Pruebas unitarias

```python
# tests.py

from django.test import TestCase
from .forms import UserRegistrationForm

class UserRegistrationFormTest(TestCase):

    def test_form_fields(self):
        form = UserRegistrationForm()
        self.assertIn('username', form.fields)
        self.assertIn('email', form.fields)
        self.assertIn('password', form.fields)

    def test_form_helper(self):
        form = UserRegistrationForm()
        self.assertIsNotNone(form.helper)
        self.assertEqual(form.helper.form_method, 'post')
```

## 4. Mejores prácticas y consideraciones de diseño

### Usabilidad

- **Consistencia:** Mantén una apariencia consistente en todos los formularios de la aplicación.
- **Accesibilidad:** Asegúrate de que los formularios sean accesibles para todos los usuarios, incluyendo aquellos con discapacidades.

### Rendimiento

- **Carga asíncrona:** Considera cargar formularios de manera asíncrona para mejorar el rendimiento.
- **Minimización de recursos:** Utiliza solo los recursos necesarios para evitar sobrecargar la página.

### Mantenibilidad

- **Modularidad:** Mantén el código modular y reutilizable.
- **Documentación:** Documenta el código y las configuraciones para facilitar el mantenimiento.

### Compatibilidad

- **Pruebas cruzadas:** Realiza pruebas en diferentes navegadores y dispositivos para asegurar la compatibilidad.
- **Actualizaciones:** Mantén Crispy Forms y sus dependencias actualizadas para evitar problemas de compatibilidad.

### Seguridad

- **Protección CSRF:** Asegúrate de incluir tokens CSRF en los formularios.
- **Validación:** Valida y sanitiza todos los datos de entrada para prevenir ataques de inyección.

### Depuración y solución de problemas

- **Logs:** Utiliza logs para rastrear y solucionar problemas.
- **Pruebas:** Implementa pruebas unitarias y funcionales para asegurar que los formularios funcionen correctamente.
- **Feedback del usuario:** Proporciona mensajes de error claros y útiles para los usuarios.

Con esta unidad, hemos cubierto la integración de Django Crispy Forms para mejorar la UI de los formularios en nuestro sistema de carrito de compras. Siguiendo estos pasos y mejores prácticas, podrás crear formularios atractivos y funcionales que mejoren la experiencia del usuario en tu aplicación Django.

# Unidad: Serializers para Modelos Principales

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en el desarrollo y uso de serializers para los modelos principales en una API REST utilizando Django REST Framework (DRF). Los serializers son componentes esenciales en DRF que permiten convertir instancias de modelos Django en formatos de datos JSON, XML, o cualquier otro formato, y viceversa. Esto es fundamental para la comunicación entre el frontend y el backend en aplicaciones web modernas.

### Objetivos de Aprendizaje

1. Comprender el propósito y la funcionalidad de los serializers en Django REST Framework.
2. Aprender a crear y configurar serializers para modelos principales en un sistema de carrito de compras.
3. Implementar serializers personalizados para manejar casos de uso específicos.
4. Realizar validaciones y transformaciones de datos utilizando serializers.
5. Integrar serializers con vistas y endpoints de la API.
6. Aplicar mejores prácticas y consideraciones de diseño al trabajar con serializers.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

En un sistema de carrito de compras, los modelos principales suelen incluir `Producto`, `Carrito`, `ItemCarrito`, y `Usuario`. Los serializers para estos modelos deben manejar la serialización y deserialización de datos, así como realizar validaciones y transformaciones necesarias para asegurar la integridad y consistencia de los datos.

#### Modelo Producto
- **Campos**: id, nombre, descripción, precio, stock, categoría.
- **Funcionalidad del Serializer**: Convertir instancias de Producto a JSON y viceversa, validar datos de entrada.

#### Modelo Carrito
- **Campos**: id, usuario, items, total.
- **Funcionalidad del Serializer**: Convertir instancias de Carrito a JSON y viceversa, calcular el total del carrito.

#### Modelo ItemCarrito
- **Campos**: id, carrito, producto, cantidad.
- **Funcionalidad del Serializer**: Convertir instancias de ItemCarrito a JSON y viceversa, validar la cantidad disponible en stock.

#### Modelo Usuario
- **Campos**: id, nombre, email, contraseña.
- **Funcionalidad del Serializer**: Convertir instancias de Usuario a JSON y viceversa, manejar la creación y actualización de usuarios de manera segura.

### Casos de Uso

1. **Crear un Producto**: Un administrador puede crear un nuevo producto proporcionando los datos necesarios.
2. **Actualizar un Producto**: Un administrador puede actualizar la información de un producto existente.
3. **Ver Productos**: Los usuarios pueden ver la lista de productos disponibles.
4. **Agregar Item al Carrito**: Un usuario puede agregar un producto a su carrito.
5. **Ver Carrito**: Un usuario puede ver los items en su carrito y el total.
6. **Actualizar Item del Carrito**: Un usuario puede cambiar la cantidad de un producto en su carrito.
7. **Eliminar Item del Carrito**: Un usuario puede eliminar un producto de su carrito.
8. **Registrar Usuario**: Un nuevo usuario puede registrarse proporcionando su información.

### Diagramas de Flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Crear Producto]
    B --> C[Validar Datos]
    C --> D[Guardar Producto]
    D --> E[Producto Creado]

    A --> F[Actualizar Producto]
    F --> G[Validar Datos]
    G --> H[Actualizar Producto]
    H --> I[Producto Actualizado]

    A --> J[Ver Productos]
    J --> K[Obtener Lista de Productos]
    K --> L[Mostrar Productos]

    A --> M[Agregar Item al Carrito]
    M --> N[Validar Stock]
    N --> O[Agregar Item]
    O --> P[Item Agregado]

    A --> Q[Ver Carrito]
    Q --> R[Obtener Items del Carrito]
    R --> S[Calcular Total]
    S --> T[Mostrar Carrito]

    A --> U[Actualizar Item del Carrito]
    U --> V[Validar Stock]
    V --> W[Actualizar Item]
    W --> X[Item Actualizado]

    A --> Y[Eliminar Item del Carrito]
    Y --> Z[Eliminar Item]
    Z --> AA[Item Eliminado]

    A --> AB[Registrar Usuario]
    AB --> AC[Validar Datos]
    AC --> AD[Guardar Usuario]
    AD --> AE[Usuario Registrado]
```

### Requisitos No Funcionales

1. **Rendimiento**: Los serializers deben ser eficientes en términos de tiempo de procesamiento y uso de memoria.
2. **Seguridad**: Los serializers deben manejar la validación de datos de manera segura para evitar inyecciones y otros ataques.
3. **Escalabilidad**: La implementación debe ser capaz de manejar un gran número de solicitudes concurrentes.
4. **Mantenibilidad**: El código debe ser claro y bien documentado para facilitar futuras modificaciones y mantenimiento.

## Implementación en Python

### Explicación Paso a Paso del Código

#### Instalación de Django y Django REST Framework

Antes de comenzar con la implementación, asegúrate de tener Django y Django REST Framework instalados. Puedes instalarlos utilizando pip:

```sh
pip install django djangorestframework
```

#### Configuración Inicial

Crea un nuevo proyecto de Django y una aplicación para el carrito de compras:

```sh
django-admin startproject carrito_compras
cd carrito_compras
django-admin startapp tienda
```

Añade `rest_framework` y `tienda` a la lista de aplicaciones instaladas en `settings.py`:

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'tienda',
]
```

#### Modelos

Define los modelos principales en `models.py` de la aplicación `tienda`:

```python
from django.db import models
from django.contrib.auth.models import User

class Categoria(models.Model):
    nombre = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE)

    def __str__(self):
        return self.nombre

class Carrito(models.Model):
    usuario = models.OneToOneField(User, on_delete=models.CASCADE)
    total = models.DecimalField(max_digits=10, decimal_places=2, default=0.00)

class ItemCarrito(models.Model):
    carrito = models.ForeignKey(Carrito, related_name='items', on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()

    def __str__(self):
        return f'{self.cantidad} x {self.producto.nombre}'
```

#### Serializers

Crea un archivo `serializers.py` en la aplicación `tienda` y define los serializers para los modelos:

```python
from rest_framework import serializers
from .models import Producto, Carrito, ItemCarrito, Categoria
from django.contrib.auth.models import User

class CategoriaSerializer(serializers.ModelSerializer):
    class Meta:
        model = Categoria
        fields = ['id', 'nombre']

class ProductoSerializer(serializers.ModelSerializer):
    categoria = CategoriaSerializer()

    class Meta:
        model = Producto
        fields = ['id', 'nombre', 'descripcion', 'precio', 'stock', 'categoria']

    def create(self, validated_data):
        categoria_data = validated_data.pop('categoria')
        categoria, created = Categoria.objects.get_or_create(**categoria_data)
        producto = Producto.objects.create(categoria=categoria, **validated_data)
        return producto

    def update(self, instance, validated_data):
        categoria_data = validated_data.pop('categoria')
        categoria, created = Categoria.objects.get_or_create(**categoria_data)
        instance.nombre = validated_data.get('nombre', instance.nombre)
        instance.descripcion = validated_data.get('descripcion', instance.descripcion)
        instance.precio = validated_data.get('precio', instance.precio)
        instance.stock = validated_data.get('stock', instance.stock)
        instance.categoria = categoria
        instance.save()
        return instance

class ItemCarritoSerializer(serializers.ModelSerializer):
    producto = ProductoSerializer()

    class Meta:
        model = ItemCarrito
        fields = ['id', 'producto', 'cantidad']

    def create(self, validated_data):
        producto_data = validated_data.pop('producto')
        producto = Producto.objects.get(**producto_data)
        item_carrito = ItemCarrito.objects.create(producto=producto, **validated_data)
        return item_carrito

class CarritoSerializer(serializers.ModelSerializer):
    items = ItemCarritoSerializer(many=True)

    class Meta:
        model = Carrito
        fields = ['id', 'usuario', 'items', 'total']

    def create(self, validated_data):
        items_data = validated_data.pop('items')
        carrito = Carrito.objects.create(**validated_data)
        for item_data in items_data:
            ItemCarrito.objects.create(carrito=carrito, **item_data)
        return carrito

    def update(self, instance, validated_data):
        items_data = validated_data.pop('items')
        instance.usuario = validated_data.get('usuario', instance.usuario)
        instance.total = validated_data.get('total', instance.total)
        instance.save()

        for item_data in items_data:
            item = ItemCarrito.objects.get(id=item_data['id'])
            item.producto = item_data.get('producto', item.producto)
            item.cantidad = item_data.get('cantidad', item.cantidad)
            item.save()
        return instance

class UsuarioSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'username', 'email', 'password']
        extra_kwargs = {'password': {'write_only': True}}

    def create(self, validated_data):
        user = User.objects.create_user(**validated_data)
        return user
```

### Código Fuente Completo y Comentado

```python
# serializers.py

from rest_framework import serializers
from .models import Producto, Carrito, ItemCarrito, Categoria
from django.contrib.auth.models import User

class CategoriaSerializer(serializers.ModelSerializer):
    class Meta:
        model = Categoria
        fields = ['id', 'nombre']

class ProductoSerializer(serializers.ModelSerializer):
    categoria = CategoriaSerializer()

    class Meta:
        model = Producto
        fields = ['id', 'nombre', 'descripcion', 'precio', 'stock', 'categoria']

    def create(self, validated_data):
        categoria_data = validated_data.pop('categoria')
        categoria, created = Categoria.objects.get_or_create(**categoria_data)
        producto = Producto.objects.create(categoria=categoria, **validated_data)
        return producto

    def update(self, instance, validated_data):
        categoria_data = validated_data.pop('categoria')
        categoria, created = Categoria.objects.get_or_create(**categoria_data)
        instance.nombre = validated_data.get('nombre', instance.nombre)
        instance.descripcion = validated_data.get('descripcion', instance.descripcion)
        instance.precio = validated_data.get('precio', instance.precio)
        instance.stock = validated_data.get('stock', instance.stock)
        instance.categoria = categoria
        instance.save()
        return instance

class ItemCarritoSerializer(serializers.ModelSerializer):
    producto = ProductoSerializer()

    class Meta:
        model = ItemCarrito
        fields = ['id', 'producto', 'cantidad']

    def create(self, validated_data):
        producto_data = validated_data.pop('producto')
        producto = Producto.objects.get(**producto_data)
        item_carrito = ItemCarrito.objects.create(producto=producto, **validated_data)
        return item_carrito

class CarritoSerializer(serializers.ModelSerializer):
    items = ItemCarritoSerializer(many=True)

    class Meta:
        model = Carrito
        fields = ['id', 'usuario', 'items', 'total']

    def create(self, validated_data):
        items_data = validated_data.pop('items')
        carrito = Carrito.objects.create(**validated_data)
        for item_data in items_data:
            ItemCarrito.objects.create(carrito=carrito, **item_data)
        return carrito

    def update(self, instance, validated_data):
        items_data = validated_data.pop('items')
        instance.usuario = validated_data.get('usuario', instance.usuario)
        instance.total = validated_data.get('total', instance.total)
        instance.save()

        for item_data in items_data:
            item = ItemCarrito.objects.get(id=item_data['id'])
            item.producto = item_data.get('producto', item.producto)
            item.cantidad = item_data.get('cantidad', item.cantidad)
            item.save()
        return instance

class UsuarioSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'username', 'email', 'password']
        extra_kwargs = {'password': {'write_only': True}}

    def create(self, validated_data):
        user = User.objects.create_user(**validated_data)
        return user
```

### Ejemplos de Uso y Pruebas Unitarias

Para probar los serializers, puedes utilizar el shell de Django o escribir pruebas unitarias. Aquí hay un ejemplo de cómo probar los serializers en el shell de Django:

```sh
python manage.py shell
```

```python
from tienda.models import Producto, Categoria, Carrito, ItemCarrito
from tienda.serializers import ProductoSerializer, CarritoSerializer, ItemCarritoSerializer, UsuarioSerializer
from django.contrib.auth.models import User

# Crear una categoría
categoria_data = {'nombre': 'Electrónica'}
categoria_serializer = CategoriaSerializer(data=categoria_data)
categoria_serializer.is_valid(raise_exception=True)
categoria = categoria_serializer.save()

# Crear un producto
producto_data = {
    'nombre': 'Laptop',
    'descripcion': 'Laptop de alta gama',
    'precio': 1500.00,
    'stock': 10,
    'categoria': categoria_data
}
producto_serializer = ProductoSerializer(data=producto_data)
producto_serializer.is_valid(raise_exception=True)
producto = producto_serializer.save()

# Crear un usuario
usuario_data = {'username': 'usuario1', 'email': 'usuario1@example.com', 'password': 'password123'}
usuario_serializer = UsuarioSerializer(data=usuario_data)
usuario_serializer.is_valid(raise_exception=True)
usuario = usuario_serializer.save()

# Crear un carrito
carrito_data = {'usuario': usuario.id, 'total': 0.00}
carrito_serializer = CarritoSerializer(data=carrito_data)
carrito_serializer.is_valid(raise_exception=True)
carrito = carrito_serializer.save()

# Agregar un item al carrito
item_carrito_data = {'carrito': carrito.id, 'producto': producto_data, 'cantidad': 1}
item_carrito_serializer = ItemCarritoSerializer(data=item_carrito_data)
item_carrito_serializer.is_valid(raise_exception=True)
item_carrito = item_carrito_serializer.save()

# Verificar el carrito
carrito_serializer = CarritoSerializer(carrito)
print(carrito_serializer.data)
```

### Pruebas Unitarias

Crea un archivo `tests.py` en la aplicación `tienda` y añade las siguientes pruebas unitarias:

```python
from django.test import TestCase
from tienda.models import Producto, Categoria, Carrito, ItemCarrito
from tienda.serializers import ProductoSerializer, CarritoSerializer, ItemCarritoSerializer, UsuarioSerializer
from django.contrib.auth.models import User

class SerializerTestCase(TestCase):
    def setUp(self):
        self.categoria = Categoria.objects.create(nombre='Electrónica')
        self.producto = Producto.objects.create(
            nombre='Laptop',
            descripcion='Laptop de alta gama',
            precio=1500.00,
            stock=10,
            categoria=self.categoria
        )
        self.usuario = User.objects.create_user(username='usuario1', email='usuario1@example.com', password='password123')
        self.carrito = Carrito.objects.create(usuario=self.usuario, total=0.00)
        self.item_carrito = ItemCarrito.objects.create(carrito=self.carrito, producto=self.producto, cantidad=1)

    def test_producto_serializer(self):
        serializer = ProductoSerializer(self.producto)
        data = serializer.data
        self.assertEqual(data['nombre'], 'Laptop')
        self.assertEqual(data['descripcion'], 'Laptop de alta gama')
        self.assertEqual(data['precio'], '1500.00')
        self.assertEqual(data['stock'], 10)
        self.assertEqual(data['categoria']['nombre'], 'Electrónica')

    def test_carrito_serializer(self):
        serializer = CarritoSerializer(self.carrito)
        data = serializer.data
        self.assertEqual(data['usuario'], self.usuario.id)
        self.assertEqual(data['total'], '0.00')
        self.assertEqual(len(data['items']), 1)

    def test_item_carrito_serializer(self):
        serializer = ItemCarritoSerializer(self.item_carrito)
        data = serializer.data
        self.assertEqual(data['producto']['nombre'], 'Laptop')
        self.assertEqual(data['cantidad'], 1)

    def test_usuario_serializer(self):
        serializer = UsuarioSerializer(self.usuario)
        data = serializer.data
        self.assertEqual(data['username'], 'usuario1')
        self.assertEqual(data['email'], 'usuario1@example.com')
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Validaciones**: Realiza validaciones exhaustivas en los serializers para asegurar la integridad de los datos. Utiliza validadores personalizados cuando sea necesario.
2. **Optimización**: Evita consultas innecesarias a la base de datos utilizando select_related y prefetch_related en los serializers.
3. **Seguridad**: Asegúrate de que los datos sensibles, como contraseñas, se manejen de manera segura. Utiliza el campo `write_only` para evitar exponer datos sensibles.
4. **Reutilización**: Crea serializers reutilizables y componibles para evitar duplicación de código.
5. **Documentación**: Documenta claramente el propósito y la funcionalidad de cada serializer para facilitar el mantenimiento y la colaboración.
6. **Pruebas**: Escribe pruebas unitarias para cada serializer para asegurar que funcionen correctamente y manejen todos los casos de borde.

Con esta unidad, has aprendido a crear y utilizar serializers en Django REST Framework para los modelos principales de un sistema de carrito de compras. Los serializers son una herramienta poderosa para manejar la serialización y deserialización de datos, así como para realizar validaciones y transformaciones necesarias. Aplicando las mejores prácticas y consideraciones de diseño, puedes construir una API REST robusta y eficiente.

# Unidad: ViewSets y Routers para la API

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, profundizaremos en el uso de ViewSets y Routers en Django REST framework (DRF) para construir una API REST eficiente y escalable para un sistema de carrito de compras. Los ViewSets y Routers son componentes esenciales que permiten simplificar y organizar el código de nuestras vistas y rutas de la API, respectivamente. Al final de esta unidad, los lectores serán capaces de:

1. Comprender la funcionalidad y ventajas de los ViewSets y Routers en DRF.
2. Implementar ViewSets para manejar operaciones CRUD de manera eficiente.
3. Configurar Routers para enrutar automáticamente las URLs de la API.
4. Aplicar mejores prácticas y consideraciones de diseño para optimizar el rendimiento y la mantenibilidad de la API.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

En esta sección, describiremos cómo los ViewSets y Routers se integran en el sistema de carrito de compras para manejar las operaciones CRUD (Crear, Leer, Actualizar, Eliminar) de los recursos principales, como productos, carritos y órdenes.

#### ViewSets

Un ViewSet es una clase que combina la lógica de múltiples vistas en una sola clase. En lugar de definir vistas individuales para cada operación (listar, crear, actualizar, eliminar), un ViewSet permite agrupar estas operaciones en una sola clase, lo que reduce la redundancia y mejora la organización del código.

#### Routers

Un Router es una clase que automáticamente enruta las URLs de la API a los métodos correspondientes de los ViewSets. Los Routers simplifican la configuración de las rutas y aseguran que las URLs de la API sigan un patrón coherente y predecible.

### Casos de Uso

1. **Gestión de Productos:**
   - **Crear Producto:** Permitir a los administradores agregar nuevos productos al catálogo.
   - **Listar Productos:** Permitir a los usuarios ver todos los productos disponibles.
   - **Actualizar Producto:** Permitir a los administradores modificar los detalles de un producto existente.
   - **Eliminar Producto:** Permitir a los administradores eliminar un producto del catálogo.

2. **Gestión de Carritos:**
   - **Crear Carrito:** Permitir a los usuarios crear un nuevo carrito de compras.
   - **Listar Carritos:** Permitir a los usuarios ver sus carritos de compras.
   - **Actualizar Carrito:** Permitir a los usuarios agregar o eliminar productos de su carrito.
   - **Eliminar Carrito:** Permitir a los usuarios eliminar un carrito de compras.

3. **Gestión de Órdenes:**
   - **Crear Orden:** Permitir a los usuarios realizar una compra y crear una nueva orden.
   - **Listar Órdenes:** Permitir a los usuarios ver sus órdenes pasadas.
   - **Actualizar Orden:** Permitir a los administradores actualizar el estado de una orden.
   - **Eliminar Orden:** Permitir a los administradores cancelar una orden.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Crear Producto]
    A --> C[Listar Productos]
    A --> D[Actualizar Producto]
    A --> E[Eliminar Producto]
    A --> F[Crear Carrito]
    A --> G[Listar Carritos]
    A --> H[Actualizar Carrito]
    A --> I[Eliminar Carrito]
    A --> J[Crear Orden]
    A --> K[Listar Órdenes]
    A --> L[Actualizar Orden]
    A --> M[Eliminar Orden]
```

### Requisitos No Funcionales

1. **Rendimiento:**
   - La API debe ser capaz de manejar múltiples solicitudes concurrentes sin degradar el rendimiento.
   - Las operaciones CRUD deben ser rápidas y eficientes.

2. **Seguridad:**
   - La API debe implementar autenticación y autorización para proteger los recursos.
   - Los datos sensibles deben ser manejados de manera segura.

3. **Escalabilidad:**
   - La arquitectura de la API debe permitir la fácil adición de nuevos recursos y funcionalidades.
   - La API debe ser capaz de escalar horizontalmente para manejar un aumento en la carga de trabajo.

4. **Mantenibilidad:**
   - El código debe ser modular y seguir las mejores prácticas de desarrollo para facilitar el mantenimiento y la evolución del sistema.
   - La documentación del código debe ser clara y completa.

## Implementación en Python

### Explicación Paso a Paso del Código

#### Instalación de Django REST framework

Antes de comenzar con la implementación, asegúrate de tener Django y Django REST framework instalados. Puedes instalarlos usando pip:

```bash
pip install django djangorestframework
```

#### Configuración Inicial

Primero, asegúrate de que `rest_framework` esté incluido en la lista de aplicaciones instaladas en tu archivo `settings.py`:

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    ...
]
```

#### Definición de Modelos

Para esta unidad, asumiremos que ya tienes definidos los modelos `Producto`, `Carrito` y `Orden`. Aquí hay un ejemplo simplificado:

```python
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)

class Carrito(models.Model):
    productos = models.ManyToManyField(Producto)
    total = models.DecimalField(max_digits=10, decimal_places=2, default=0.00)

class Orden(models.Model):
    carrito = models.OneToOneField(Carrito, on_delete=models.CASCADE)
    fecha = models.DateTimeField(auto_now_add=True)
    estado = models.CharField(max_length=20, choices=[('pendiente', 'Pendiente'), ('completada', 'Completada')])
```

#### Serializers

A continuación, definimos los serializers para estos modelos. Los serializers se encargan de convertir las instancias de los modelos a formatos JSON y viceversa.

```python
from rest_framework import serializers
from .models import Producto, Carrito, Orden

class ProductoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Producto
        fields = '__all__'

class CarritoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Carrito
        fields = '__all__'

class OrdenSerializer(serializers.ModelSerializer):
    class Meta:
        model = Orden
        fields = '__all__'
```

#### ViewSets

Ahora, definimos los ViewSets para manejar las operaciones CRUD de los modelos.

```python
from rest_framework import viewsets
from .models import Producto, Carrito, Orden
from .serializers import ProductoSerializer, CarritoSerializer, OrdenSerializer

class ProductoViewSet(viewsets.ModelViewSet):
    queryset = Producto.objects.all()
    serializer_class = ProductoSerializer

class CarritoViewSet(viewsets.ModelViewSet):
    queryset = Carrito.objects.all()
    serializer_class = CarritoSerializer

class OrdenViewSet(viewsets.ModelViewSet):
    queryset = Orden.objects.all()
    serializer_class = OrdenSerializer
```

#### Routers

Finalmente, configuramos los Routers para enrutar automáticamente las URLs de la API a los métodos correspondientes de los ViewSets.

```python
from rest_framework.routers import DefaultRouter
from django.urls import path, include
from .views import ProductoViewSet, CarritoViewSet, OrdenViewSet

router = DefaultRouter()
router.register(r'productos', ProductoViewSet)
router.register(r'carritos', CarritoViewSet)
router.register(r'ordenes', OrdenViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```

### Código Fuente Completo y Comentado

A continuación, se presenta el código fuente completo y comentado para la implementación de los ViewSets y Routers en la API.

#### models.py

```python
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.nombre

class Carrito(models.Model):
    productos = models.ManyToManyField(Producto)
    total = models.DecimalField(max_digits=10, decimal_places=2, default=0.00)

    def __str__(self):
        return f"Carrito {self.id}"

class Orden(models.Model):
    carrito = models.OneToOneField(Carrito, on_delete=models.CASCADE)
    fecha = models.DateTimeField(auto_now_add=True)
    estado = models.CharField(max_length=20, choices=[('pendiente', 'Pendiente'), ('completada', 'Completada')])

    def __str__(self):
        return f"Orden {self.id}"
```

#### serializers.py

```python
from rest_framework import serializers
from .models import Producto, Carrito, Orden

class ProductoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Producto
        fields = '__all__'

class CarritoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Carrito
        fields = '__all__'

class OrdenSerializer(serializers.ModelSerializer):
    class Meta:
        model = Orden
        fields = '__all__'
```

#### views.py

```python
from rest_framework import viewsets
from .models import Producto, Carrito, Orden
from .serializers import ProductoSerializer, CarritoSerializer, OrdenSerializer

class ProductoViewSet(viewsets.ModelViewSet):
    queryset = Producto.objects.all()
    serializer_class = ProductoSerializer

class CarritoViewSet(viewsets.ModelViewSet):
    queryset = Carrito.objects.all()
    serializer_class = CarritoSerializer

class OrdenViewSet(viewsets.ModelViewSet):
    queryset = Orden.objects.all()
    serializer_class = OrdenSerializer
```

#### urls.py

```python
from rest_framework.routers import DefaultRouter
from django.urls import path, include
from .views import ProductoViewSet, CarritoViewSet, OrdenViewSet

router = DefaultRouter()
router.register(r'productos', ProductoViewSet)
router.register(r'carritos', CarritoViewSet)
router.register(r'ordenes', OrdenViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```

### Ejemplos de Uso y Pruebas Unitarias

Para probar la funcionalidad de la API, puedes usar herramientas como Postman o cURL. A continuación, se presentan algunos ejemplos de uso y pruebas unitarias.

#### Ejemplo de Uso con cURL

1. **Crear Producto:**

```bash
curl -X POST http://localhost:8000/productos/ -H "Content-Type: application/json" -d '{"nombre": "Producto 1", "descripcion": "Descripción del Producto 1", "precio": 100.00}'
```

2. **Listar Productos:**

```bash
curl -X GET http://localhost:8000/productos/
```

3. **Actualizar Producto:**

```bash
curl -X PUT http://localhost:8000/productos/1/ -H "Content-Type: application/json" -d '{"nombre": "Producto 1 Actualizado", "descripcion": "Descripción actualizada", "precio": 150.00}'
```

4. **Eliminar Producto:**

```bash
curl -X DELETE http://localhost:8000/productos/1/
```

#### Pruebas Unitarias

Para asegurar que la API funciona correctamente, es importante escribir pruebas unitarias. Aquí hay un ejemplo de cómo hacerlo usando el framework de pruebas de Django.

```python
from django.test import TestCase
from rest_framework.test import APIClient
from rest_framework import status
from .models import Producto

class ProductoAPITestCase(TestCase):
    def setUp(self):
        self.client = APIClient()
        self.producto_data = {'nombre': 'Producto Test', 'descripcion': 'Descripción Test', 'precio': 50.00}
        self.producto = Producto.objects.create(**self.producto_data)

    def test_crear_producto(self):
        response = self.client.post('/productos/', self.producto_data, format='json')
        self.assertEqual(response.status_code, status.HTTP_201_CREATED)

    def test_listar_productos(self):
        response = self.client.get('/productos/')
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertEqual(len(response.data), 1)

    def test_actualizar_producto(self):
        updated_data = {'nombre': 'Producto Actualizado', 'descripcion': 'Descripción Actualizada', 'precio': 60.00}
        response = self.client.put(f'/productos/{self.producto.id}/', updated_data, format='json')
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.producto.refresh_from_db()
        self.assertEqual(self.producto.nombre, updated_data['nombre'])

    def test_eliminar_producto(self):
        response = self.client.delete(f'/productos/{self.producto.id}/')
        self.assertEqual(response.status_code, status.HTTP_204_NO_CONTENT)
        self.assertEqual(Producto.objects.count(), 0)
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Modularidad:**
   - Mantén el código modular separando las definiciones de modelos, serializers, views y rutas en archivos separados. Esto facilita la mantenibilidad y escalabilidad del proyecto.

2. **Autenticación y Autorización:**
   - Implementa mecanismos de autenticación y autorización para proteger los recursos de la API. Django REST framework ofrece varios métodos de autenticación, como tokens y JWT.

3. **Validación de Datos:**
   - Asegúrate de que los serializers validen correctamente los datos de entrada para evitar errores y garantizar la integridad de los datos.

4. **Documentación:**
   - Documenta el código y la API utilizando herramientas como Swagger o la documentación automática de Django REST framework. Esto facilita la comprensión y el uso de la API por parte de otros desarrolladores.

5. **Pruebas:**
   - Escribe pruebas unitarias y de integración para asegurar que la API funcione correctamente y detectar posibles errores antes de que lleguen a producción.

6. **Optimización de Consultas:**
   - Utiliza técnicas de optimización de consultas, como `select_related` y `prefetch_related`, para reducir el número de consultas a la base de datos y mejorar el rendimiento de la API.

7. **Manejo de Errores:**
   - Implementa un manejo adecuado de errores para proporcionar respuestas claras y útiles a los clientes de la API en caso de fallos.

8. **Versionado de la API:**
   - Considera versionar la API para facilitar la introducción de cambios y nuevas funcionalidades sin romper la compatibilidad con versiones anteriores.

9. **Seguridad:**
   - Asegúrate de que la API esté protegida contra vulnerabilidades comunes, como inyecciones SQL, ataques CSRF y XSS. Utiliza las herramientas y configuraciones de seguridad proporcionadas por Django y Django REST framework.

10. **Escalabilidad:**
    - Diseña la API pensando en la escalabilidad. Utiliza técnicas como la paginación para manejar grandes volúmenes de datos y considera el uso de caché para mejorar el rendimiento.

Siguiendo estas mejores prácticas y consideraciones de diseño, podrás construir una API REST robusta, segura y eficiente para tu sistema de carrito de compras con Django y Django REST framework.

# Unidad: Autenticación y permisos en la API

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, profundizaremos en el tema de la autenticación y permisos en la API, específicamente en el contexto de un sistema de carrito de compras desarrollado con Django y Django REST framework (DRF). La autenticación y los permisos son componentes críticos para garantizar que solo los usuarios autorizados puedan acceder y manipular los recursos de la API. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender los conceptos fundamentales de autenticación y permisos en Django REST framework.
2. Implementar distintos métodos de autenticación, como token-based y session-based.
3. Configurar permisos personalizados para controlar el acceso a los endpoints de la API.
4. Integrar y probar la autenticación y los permisos en el sistema de carrito de compras.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La autenticación y permisos en la API del sistema de carrito de compras deben garantizar que solo los usuarios autenticados puedan realizar ciertas acciones, como agregar productos al carrito, realizar compras, y gestionar sus perfiles. Los objetivos específicos incluyen:

- Implementar autenticación basada en tokens para la API.
- Configurar permisos para restringir el acceso a ciertos endpoints según el rol del usuario (por ejemplo, administrador, cliente).
- Proveer endpoints para el registro y autenticación de usuarios.
- Asegurar que las operaciones sensibles requieran autenticación y permisos adecuados.

### b. Casos de uso

#### Caso de uso 1: Registro de usuario

**Actor:** Usuario no autenticado

**Descripción:** Un usuario se registra en el sistema proporcionando su información básica (nombre, email, contraseña).

**Precondiciones:** Ninguna

**Flujo principal:**
1. El usuario envía una solicitud POST al endpoint `/api/register/` con su información.
2. El sistema valida la información y crea una nueva cuenta de usuario.
3. El sistema devuelve una respuesta de éxito con un token de autenticación.

**Postcondiciones:** El usuario está registrado y autenticado.

#### Caso de uso 2: Autenticación de usuario

**Actor:** Usuario registrado

**Descripción:** Un usuario registrado se autentica en el sistema proporcionando sus credenciales (email y contraseña).

**Precondiciones:** El usuario debe estar registrado.

**Flujo principal:**
1. El usuario envía una solicitud POST al endpoint `/api/login/` con sus credenciales.
2. El sistema valida las credenciales.
3. El sistema devuelve una respuesta de éxito con un token de autenticación.

**Postcondiciones:** El usuario está autenticado y recibe un token de autenticación.

#### Caso de uso 3: Agregar producto al carrito

**Actor:** Usuario autenticado

**Descripción:** Un usuario autenticado agrega un producto a su carrito de compras.

**Precondiciones:** El usuario debe estar autenticado y el producto debe existir.

**Flujo principal:**
1. El usuario envía una solicitud POST al endpoint `/api/cart/add/` con el ID del producto.
2. El sistema valida la autenticación del usuario y la existencia del producto.
3. El sistema agrega el producto al carrito del usuario.
4. El sistema devuelve una respuesta de éxito.

**Postcondiciones:** El producto está agregado al carrito del usuario.

### c. Diagramas de flujo (en mermaid)

```mermaid
flowchart TD
    A[Usuario no autenticado] -->|Registro| B[POST /api/register/]
    B --> C[Validar información]
    C -->|Información válida| D[Crear cuenta de usuario]
    D --> E[Generar token de autenticación]
    E --> F[Respuesta de éxito]

    G[Usuario registrado] -->|Autenticación| H[POST /api/login/]
    H --> I[Validar credenciales]
    I -->|Credenciales válidas| J[Generar token de autenticación]
    J --> K[Respuesta de éxito]

    L[Usuario autenticado] -->|Agregar producto al carrito| M[POST /api/cart/add/]
    M --> N[Validar autenticación y existencia del producto]
    N -->|Validación exitosa| O[Agregar producto al carrito]
    O --> P[Respuesta de éxito]
```

### d. Requisitos no funcionales

- **Seguridad:** Los tokens de autenticación deben ser seguros y difíciles de adivinar. La API debe protegerse contra ataques comunes como inyección SQL, XSS y CSRF.
- **Rendimiento:** La autenticación y verificación de permisos deben ser eficientes para no afectar el rendimiento general de la API.
- **Escalabilidad:** El sistema debe ser capaz de manejar un gran número de solicitudes concurrentes sin degradar el rendimiento.
- **Usabilidad:** Los endpoints de autenticación y permisos deben ser fáciles de usar y bien documentados.

## Implementación en Python

### a. Explicación paso a paso del código

1. **Configuración del proyecto y aplicaciones necesarias:**
   - Asegúrate de tener Django y Django REST framework instalados.
   - Crea una nueva aplicación llamada `authentication` para manejar el registro y autenticación de usuarios.

2. **Modelos de usuario:**
   - Utiliza el modelo de usuario predeterminado de Django o extiéndelo según sea necesario.

3. **Serializers:**
   - Crea serializers para manejar la validación y serialización de datos de usuario.

4. **Views:**
   - Implementa vistas para los endpoints de registro y autenticación.

5. **URLs:**
   - Configura las rutas para los endpoints de autenticación.

6. **Permisos personalizados:**
   - Define permisos personalizados para controlar el acceso a los endpoints de la API.

### b. Código fuente completo y comentado

#### Configuración del proyecto

```python
# settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework.authtoken',
    'authentication',
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
}
```

#### Modelos de usuario

```python
# models.py (en la aplicación authentication)
from django.contrib.auth.models import AbstractUser

class CustomUser(AbstractUser):
    # Puedes agregar campos adicionales si es necesario
    pass
```

#### Serializers

```python
# serializers.py (en la aplicación authentication)
from rest_framework import serializers
from django.contrib.auth.models import User

class RegisterSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ('username', 'password', 'email')
        extra_kwargs = {'password': {'write_only': True}}

    def create(self, validated_data):
        user = User.objects.create_user(
            username=validated_data['username'],
            password=validated_data['password'],
            email=validated_data['email']
        )
        return user

class LoginSerializer(serializers.Serializer):
    username = serializers.CharField()
    password = serializers.CharField(write_only=True)
```

#### Views

```python
# views.py (en la aplicación authentication)
from rest_framework import generics, status
from rest_framework.response import Response
from rest_framework.authtoken.models import Token
from rest_framework.permissions import AllowAny
from django.contrib.auth.models import User
from django.contrib.auth import authenticate
from .serializers import RegisterSerializer, LoginSerializer

class RegisterView(generics.CreateAPIView):
    queryset = User.objects.all()
    serializer_class = RegisterSerializer
    permission_classes = [AllowAny]

    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        user = serializer.save()
        token, created = Token.objects.get_or_create(user=user)
        return Response({
            'token': token.key,
            'user': serializer.data
        }, status=status.HTTP_201_CREATED)

class LoginView(generics.GenericAPIView):
    serializer_class = LoginSerializer
    permission_classes = [AllowAny]

    def post(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        user = authenticate(
            username=serializer.validated_data['username'],
            password=serializer.validated_data['password']
        )
        if user is not None:
            token, created = Token.objects.get_or_create(user=user)
            return Response({
                'token': token.key,
                'user': {
                    'username': user.username,
                    'email': user.email
                }
            }, status=status.HTTP_200_OK)
        return Response({'error': 'Invalid credentials'}, status=status.HTTP_400_BAD_REQUEST)
```

#### URLs

```python
# urls.py (en la aplicación authentication)
from django.urls import path
from .views import RegisterView, LoginView

urlpatterns = [
    path('register/', RegisterView.as_view(), name='register'),
    path('login/', LoginView.as_view(), name='login'),
]
```

#### Permisos personalizados

```python
# permissions.py (en la aplicación principal)
from rest_framework.permissions import BasePermission

class IsAdminOrReadOnly(BasePermission):
    def has_permission(self, request, view):
        if request.method in ['GET', 'HEAD', 'OPTIONS']:
            return True
        return request.user and request.user.is_staff
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso: Registro de usuario

```bash
curl -X POST http://127.0.0.1:8000/api/register/ \
-H "Content-Type: application/json" \
-d '{"username": "newuser", "password": "password123", "email": "newuser@example.com"}'
```

#### Ejemplo de uso: Autenticación de usuario

```bash
curl -X POST http://127.0.0.1:8000/api/login/ \
-H "Content-Type: application/json" \
-d '{"username": "newuser", "password": "password123"}'
```

#### Pruebas unitarias

```python
# tests.py (en la aplicación authentication)
from rest_framework.test import APITestCase
from django.contrib.auth.models import User
from rest_framework.authtoken.models import Token

class AuthenticationTests(APITestCase):
    def test_register_user(self):
        data = {
            'username': 'testuser',
            'password': 'testpassword',
            'email': 'testuser@example.com'
        }
        response = self.client.post('/api/register/', data)
        self.assertEqual(response.status_code, 201)
        self.assertIn('token', response.data)

    def test_login_user(self):
        user = User.objects.create_user(username='testuser', password='testpassword')
        data = {
            'username': 'testuser',
            'password': 'testpassword'
        }
        response = self.client.post('/api/login/', data)
        self.assertEqual(response.status_code, 200)
        self.assertIn('token', response.data)
```

### d. Mejores prácticas y consideraciones de diseño

1. **Seguridad:** Asegúrate de utilizar HTTPS para todas las comunicaciones con la API para proteger los tokens de autenticación. Implementa medidas adicionales como la rotación de tokens y la invalidación de tokens comprometidos.

2. **Manejo de errores:** Proporciona mensajes de error claros y útiles para los usuarios. Maneja adecuadamente las excepciones y errores comunes, como credenciales inválidas o usuarios no encontrados.

3. **Escalabilidad:** Considera el uso de soluciones de caché y balanceo de carga para manejar un gran número de solicitudes concurrentes. Utiliza bases de datos distribuidas si es necesario.

4. **Pruebas:** Implementa pruebas unitarias y de integración para asegurar que la autenticación y los permisos funcionen correctamente. Utiliza herramientas de prueba automatizadas para mantener la calidad del código.

5. **Documentación:** Proporciona documentación clara y detallada para los endpoints de autenticación y permisos. Utiliza herramientas como Swagger o Postman para generar y mantener la documentación de la API.

6. **Permisos granulares:** Define permisos granulares para controlar el acceso a los recursos de la API de manera más precisa. Considera el uso de roles y grupos de usuarios para simplificar la gestión de permisos.

7. **Auditoría y monitoreo:** Implementa mecanismos de auditoría y monitoreo para rastrear el uso de la API y detectar posibles intentos de acceso no autorizado. Utiliza herramientas de monitoreo como Prometheus y Grafana para obtener información en tiempo real sobre el rendimiento y la seguridad de la API.

En resumen, la autenticación y los permisos son componentes esenciales para cualquier API segura y robusta. Siguiendo las mejores prácticas y consideraciones de diseño, puedes asegurar que tu sistema de carrito de compras con Django REST framework sea seguro, escalable y fácil de usar.

# Unidad: Endpoints para gestión de carrito y checkout

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, abordaremos la implementación de endpoints para la gestión de carritos de compras y el proceso de checkout en una API REST utilizando Django REST Framework (DRF). Este capítulo es fundamental para completar la funcionalidad de un sistema de carrito de compras, permitiendo a los usuarios agregar, actualizar y eliminar productos en su carrito, así como proceder al pago y finalizar la compra.

### Objetivos de aprendizaje

1. Comprender la estructura y funcionalidad de los endpoints para la gestión de carritos de compras.
2. Implementar endpoints para agregar, actualizar y eliminar productos del carrito.
3. Desarrollar el proceso de checkout, incluyendo la creación de órdenes y el manejo de pagos.
4. Aplicar mejores prácticas y consideraciones de diseño en la implementación de estos endpoints.
5. Realizar pruebas unitarias para asegurar la correcta funcionalidad de los endpoints.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

La gestión de carritos de compras y el proceso de checkout son componentes esenciales en cualquier sistema de comercio electrónico. La funcionalidad principal incluye:

- **Agregar productos al carrito:** Permitir a los usuarios agregar productos a su carrito de compras.
- **Actualizar productos en el carrito:** Permitir a los usuarios modificar la cantidad de productos en su carrito.
- **Eliminar productos del carrito:** Permitir a los usuarios eliminar productos de su carrito.
- **Ver el contenido del carrito:** Permitir a los usuarios ver los productos que han agregado a su carrito.
- **Checkout:** Permitir a los usuarios proceder al pago y finalizar la compra, generando una orden.

### Casos de uso

1. **Agregar producto al carrito:**
   - **Actor:** Usuario autenticado.
   - **Descripción:** El usuario selecciona un producto y lo agrega a su carrito de compras.
   - **Precondiciones:** El usuario debe estar autenticado.
   - **Postcondiciones:** El producto se agrega al carrito del usuario.

2. **Actualizar producto en el carrito:**
   - **Actor:** Usuario autenticado.
   - **Descripción:** El usuario modifica la cantidad de un producto en su carrito.
   - **Precondiciones:** El usuario debe estar autenticado y el producto debe estar en el carrito.
   - **Postcondiciones:** La cantidad del producto en el carrito se actualiza.

3. **Eliminar producto del carrito:**
   - **Actor:** Usuario autenticado.
   - **Descripción:** El usuario elimina un producto de su carrito.
   - **Precondiciones:** El usuario debe estar autenticado y el producto debe estar en el carrito.
   - **Postcondiciones:** El producto se elimina del carrito del usuario.

4. **Ver contenido del carrito:**
   - **Actor:** Usuario autenticado.
   - **Descripción:** El usuario visualiza los productos en su carrito.
   - **Precondiciones:** El usuario debe estar autenticado.
   - **Postcondiciones:** Se muestra el contenido del carrito.

5. **Checkout:**
   - **Actor:** Usuario autenticado.
   - **Descripción:** El usuario procede al pago y finaliza la compra.
   - **Precondiciones:** El usuario debe estar autenticado y tener productos en su carrito.
   - **Postcondiciones:** Se genera una orden y se vacía el carrito del usuario.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Usuario autenticado?]
    B -- No --> C[Redirigir a login]
    B -- Sí --> D[Agregar producto al carrito]
    D --> E[Actualizar producto en el carrito]
    E --> F[Eliminar producto del carrito]
    F --> G[Ver contenido del carrito]
    G --> H[Checkout]
    H --> I[Generar orden y vaciar carrito]
    I --> J[Fin]
```

### Requisitos no funcionales

1. **Seguridad:** Los endpoints deben estar protegidos mediante autenticación y permisos adecuados para asegurar que solo los usuarios autenticados puedan acceder y modificar su propio carrito.
2. **Rendimiento:** La API debe ser capaz de manejar múltiples solicitudes concurrentes sin degradar el rendimiento.
3. **Escalabilidad:** La arquitectura debe permitir la fácil escalabilidad para manejar un aumento en el número de usuarios y transacciones.
4. **Mantenibilidad:** El código debe ser claro y bien documentado para facilitar futuras modificaciones y mantenimiento.
5. **Pruebas:** Se deben implementar pruebas unitarias y de integración para asegurar la correcta funcionalidad de los endpoints.

## Implementación en Python

### Explicación paso a paso del código

Para implementar los endpoints de gestión de carritos y checkout, utilizaremos Django REST Framework. A continuación, se describe cada paso de la implementación.

#### 1. Crear modelos para Carrito y Orden

Primero, definimos los modelos `Cart` y `Order` en `models.py`.

```python
from django.db import models
from django.contrib.auth.models import User
from shop.models import Product

class Cart(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

class CartItem(models.Model):
    cart = models.ForeignKey(Cart, related_name='items', on_delete=models.CASCADE)
    product = models.ForeignKey(Product, on_delete=models.CASCADE)
    quantity = models.PositiveIntegerField(default=1)

class Order(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

#### 2. Crear Serializers

Definimos los serializers para los modelos `Cart`, `CartItem` y `Order` en `serializers.py`.

```python
from rest_framework import serializers
from .models import Cart, CartItem, Order

class CartItemSerializer(serializers.ModelSerializer):
    class Meta:
        model = CartItem
        fields = ['id', 'product', 'quantity']

class CartSerializer(serializers.ModelSerializer):
    items = CartItemSerializer(many=True, read_only=True)

    class Meta:
        model = Cart
        fields = ['id', 'user', 'items', 'created_at', 'updated_at']

class OrderSerializer(serializers.ModelSerializer):
    class Meta:
        model = Order
        fields = ['id', 'user', 'total', 'created_at', 'updated_at']
```

#### 3. Crear ViewSets

Implementamos los ViewSets para manejar las operaciones CRUD en `views.py`.

```python
from rest_framework import viewsets, status
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
from .models import Cart, CartItem, Order
from .serializers import CartSerializer, CartItemSerializer, OrderSerializer

class CartViewSet(viewsets.ModelViewSet):
    serializer_class = CartSerializer
    permission_classes = [IsAuthenticated]

    def get_queryset(self):
        return Cart.objects.filter(user=self.request.user)

    def create(self, request, *args, **kwargs):
        cart, created = Cart.objects.get_or_create(user=request.user)
        return Response(CartSerializer(cart).data)

class CartItemViewSet(viewsets.ModelViewSet):
    serializer_class = CartItemSerializer
    permission_classes = [IsAuthenticated]

    def get_queryset(self):
        return CartItem.objects.filter(cart__user=self.request.user)

    def create(self, request, *args, **kwargs):
        cart = Cart.objects.get(user=request.user)
        product = request.data.get('product')
        quantity = request.data.get('quantity', 1)
        cart_item, created = CartItem.objects.get_or_create(cart=cart, product_id=product, defaults={'quantity': quantity})
        if not created:
            cart_item.quantity += int(quantity)
            cart_item.save()
        return Response(CartItemSerializer(cart_item).data)

    def update(self, request, *args, **kwargs):
        cart_item = self.get_object()
        cart_item.quantity = request.data.get('quantity', cart_item.quantity)
        cart_item.save()
        return Response(CartItemSerializer(cart_item).data)

    def destroy(self, request, *args, **kwargs):
        cart_item = self.get_object()
        cart_item.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)

class OrderViewSet(viewsets.ModelViewSet):
    serializer_class = OrderSerializer
    permission_classes = [IsAuthenticated]

    def get_queryset(self):
        return Order.objects.filter(user=self.request.user)

    def create(self, request, *args, **kwargs):
        cart = Cart.objects.get(user=request.user)
        total = sum(item.product.price * item.quantity for item in cart.items.all())
        order = Order.objects.create(user=request.user, total=total)
        cart.items.all().delete()
        return Response(OrderSerializer(order).data)
```

#### 4. Configurar URLs

Configuramos las URLs para los ViewSets en `urls.py`.

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import CartViewSet, CartItemViewSet, OrderViewSet

router = DefaultRouter()
router.register(r'cart', CartViewSet, basename='cart')
router.register(r'cart-items', CartItemViewSet, basename='cartitem')
router.register(r'orders', OrderViewSet, basename='order')

urlpatterns = [
    path('', include(router.urls)),
]
```

### Código fuente completo y comentado

A continuación, se presenta el código fuente completo y comentado para la implementación de los endpoints de gestión de carritos y checkout.

```python
# models.py
from django.db import models
from django.contrib.auth.models import User
from shop.models import Product

class Cart(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

class CartItem(models.Model):
    cart = models.ForeignKey(Cart, related_name='items', on_delete=models.CASCADE)
    product = models.ForeignKey(Product, on_delete=models.CASCADE)
    quantity = models.PositiveIntegerField(default=1)

class Order(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

# serializers.py
from rest_framework import serializers
from .models import Cart, CartItem, Order

class CartItemSerializer(serializers.ModelSerializer):
    class Meta:
        model = CartItem
        fields = ['id', 'product', 'quantity']

class CartSerializer(serializers.ModelSerializer):
    items = CartItemSerializer(many=True, read_only=True)

    class Meta:
        model = Cart
        fields = ['id', 'user', 'items', 'created_at', 'updated_at']

class OrderSerializer(serializers.ModelSerializer):
    class Meta:
        model = Order
        fields = ['id', 'user', 'total', 'created_at', 'updated_at']

# views.py
from rest_framework import viewsets, status
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
from .models import Cart, CartItem, Order
from .serializers import CartSerializer, CartItemSerializer, OrderSerializer

class CartViewSet(viewsets.ModelViewSet):
    serializer_class = CartSerializer
    permission_classes = [IsAuthenticated]

    def get_queryset(self):
        return Cart.objects.filter(user=self.request.user)

    def create(self, request, *args, **kwargs):
        cart, created = Cart.objects.get_or_create(user=request.user)
        return Response(CartSerializer(cart).data)

class CartItemViewSet(viewsets.ModelViewSet):
    serializer_class = CartItemSerializer
    permission_classes = [IsAuthenticated]

    def get_queryset(self):
        return CartItem.objects.filter(cart__user=self.request.user)

    def create(self, request, *args, **kwargs):
        cart = Cart.objects.get(user=request.user)
        product = request.data.get('product')
        quantity = request.data.get('quantity', 1)
        cart_item, created = CartItem.objects.get_or_create(cart=cart, product_id=product, defaults={'quantity': quantity})
        if not created:
            cart_item.quantity += int(quantity)
            cart_item.save()
        return Response(CartItemSerializer(cart_item).data)

    def update(self, request, *args, **kwargs):
        cart_item = self.get_object()
        cart_item.quantity = request.data.get('quantity', cart_item.quantity)
        cart_item.save()
        return Response(CartItemSerializer(cart_item).data)

    def destroy(self, request, *args, **kwargs):
        cart_item = self.get_object()
        cart_item.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)

class OrderViewSet(viewsets.ModelViewSet):
    serializer_class = OrderSerializer
    permission_classes = [IsAuthenticated]

    def get_queryset(self):
        return Order.objects.filter(user=self.request.user)

    def create(self, request, *args, **kwargs):
        cart = Cart.objects.get(user=request.user)
        total = sum(item.product.price * item.quantity for item in cart.items.all())
        order = Order.objects.create(user=request.user, total=total)
        cart.items.all().delete()
        return Response(OrderSerializer(order).data)

# urls.py
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import CartViewSet, CartItemViewSet, OrderViewSet

router = DefaultRouter()
router.register(r'cart', CartViewSet, basename='cart')
router.register(r'cart-items', CartItemViewSet, basename='cartitem')
router.register(r'orders', OrderViewSet, basename='order')

urlpatterns = [
    path('', include(router.urls)),
]
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplos de uso

1. **Agregar producto al carrito:**

```bash
POST /cart-items/
{
    "product": 1,
    "quantity": 2
}
```

2. **Actualizar producto en el carrito:**

```bash
PUT /cart-items/1/
{
    "quantity": 3
}
```

3. **Eliminar producto del carrito:**

```bash
DELETE /cart-items/1/
```

4. **Ver contenido del carrito:**

```bash
GET /cart/
```

5. **Checkout:**

```bash
POST /orders/
```

#### Pruebas unitarias

A continuación, se presentan algunas pruebas unitarias para los endpoints implementados.

```python
from django.urls import reverse
from rest_framework import status
from rest_framework.test import APITestCase
from django.contrib.auth.models import User
from shop.models import Product
from .models import Cart, CartItem, Order

class CartTests(APITestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='testuser', password='testpass')
        self.product = Product.objects.create(name='Test Product', price=100.00)
        self.client.login(username='testuser', password='testpass')

    def test_add_product_to_cart(self):
        url = reverse('cartitem-list')
        data = {'product': self.product.id, 'quantity': 2}
        response = self.client.post(url, data, format='json')
        self.assertEqual(response.status_code, status.HTTP_201_CREATED)
        self.assertEqual(CartItem.objects.count(), 1)
        self.assertEqual(CartItem.objects.get().quantity, 2)

    def test_update_product_in_cart(self):
        cart = Cart.objects.create(user=self.user)
        cart_item = CartItem.objects.create(cart=cart, product=self.product, quantity=2)
        url = reverse('cartitem-detail', args=[cart_item.id])
        data = {'quantity': 3}
        response = self.client.put(url, data, format='json')
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertEqual(CartItem.objects.get().quantity, 3)

    def test_delete_product_from_cart(self):
        cart = Cart.objects.create(user=self.user)
        cart_item = CartItem.objects.create(cart=cart, product=self.product, quantity=2)
        url = reverse('cartitem-detail', args=[cart_item.id])
        response = self.client.delete(url)
        self.assertEqual(response.status_code, status.HTTP_204_NO_CONTENT)
        self.assertEqual(CartItem.objects.count(), 0)

    def test_checkout(self):
        cart = Cart.objects.create(user=self.user)
        CartItem.objects.create(cart=cart, product=self.product, quantity=2)
        url = reverse('order-list')
        response = self.client.post(url, format='json')
        self.assertEqual(response.status_code, status.HTTP_201_CREATED)
        self.assertEqual(Order.objects.count(), 1)
        self.assertEqual(CartItem.objects.count(), 0)

```

## Mejores prácticas y consideraciones de diseño

### Seguridad

1. **Autenticación y permisos:** Asegurarse de que solo los usuarios autenticados puedan acceder y modificar su propio carrito.
2. **Validación de datos:** Validar los datos de entrada para evitar inyecciones de código y otros ataques.

### Rendimiento

1. **Consultas eficientes:** Utilizar consultas eficientes para evitar sobrecargar la base de datos.
2. **Caché:** Implementar caché para mejorar el rendimiento de la API.

### Escalabilidad

1. **Microservicios:** Considerar la arquitectura de microservicios para permitir la escalabilidad horizontal.
2. **Balanceo de carga:** Implementar balanceo de carga para distribuir las solicitudes entre múltiples servidores.

### Mantenibilidad

1. **Código limpio y documentado:** Escribir código claro y bien documentado para facilitar el mantenimiento.
2. **Pruebas:** Implementar pruebas unitarias y de integración para asegurar la correcta funcionalidad de la API.

### Depuración y solución de problemas comunes

1. **Logs:** Implementar logging para facilitar la depuración y el monitoreo de la API.
2. **Manejo de errores:** Manejar adecuadamente los errores y proporcionar mensajes de error claros y útiles.

En resumen, la implementación de endpoints para la gestión de carritos y checkout en una API REST con Django REST Framework es un proceso que requiere atención a la seguridad, rendimiento, escalabilidad y mantenibilidad. Siguiendo las mejores prácticas y realizando pruebas exhaustivas, se puede asegurar una funcionalidad robusta y eficiente para los usuarios.

# Unidad: Documentación de la API con drf-yasg

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la documentación de la API utilizando `drf-yasg`, una herramienta poderosa y flexible para generar documentación de API en Django REST Framework (DRF). La documentación de API es crucial para el desarrollo y mantenimiento de aplicaciones, ya que facilita la comprensión y el uso de la API tanto para desarrolladores internos como externos. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender la importancia de la documentación de API.
2. Instalar y configurar `drf-yasg` en un proyecto Django.
3. Generar documentación interactiva y personalizable para la API.
4. Integrar la documentación con las vistas y endpoints existentes.
5. Aplicar mejores prácticas para mantener la documentación actualizada y precisa.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es la generación automática de documentación para la API REST utilizando `drf-yasg`. Esta herramienta permite crear una interfaz interactiva basada en Swagger/OpenAPI que facilita la exploración y prueba de los endpoints disponibles en la API. La documentación generada debe ser clara, precisa y reflejar fielmente los endpoints, métodos HTTP, parámetros y respuestas de la API.

### b. Casos de uso

1. **Desarrollador interno**: Un desarrollador que trabaja en el proyecto necesita entender cómo interactuar con la API para implementar nuevas funcionalidades o corregir errores.
2. **Desarrollador externo**: Un desarrollador externo que desea integrar su aplicación con la API necesita una referencia completa y precisa de los endpoints disponibles.
3. **Tester**: Un tester necesita explorar y probar los diferentes endpoints de la API para asegurar su correcto funcionamiento.
4. **Cliente**: Un cliente que utiliza la API para sus propias aplicaciones necesita comprender cómo realizar solicitudes y qué respuestas esperar.

### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Instalación de drf-yasg]
    B --> C[Configuración de drf-yasg]
    C --> D[Generación de documentación]
    D --> E[Integración con vistas y endpoints]
    E --> F[Pruebas y validación]
    F --> G[Mejores prácticas y mantenimiento]
    G --> H[Fin]
```

### d. Requisitos no funcionales

1. **Rendimiento**: La generación de la documentación no debe afectar significativamente el rendimiento de la API.
2. **Escalabilidad**: La solución debe ser capaz de manejar un aumento en la cantidad de endpoints y usuarios sin degradar la calidad de la documentación.
3. **Seguridad**: La documentación debe estar protegida y accesible solo para usuarios autorizados.
4. **Usabilidad**: La interfaz de la documentación debe ser intuitiva y fácil de navegar.
5. **Mantenibilidad**: La configuración y el código relacionado con la documentación deben ser fáciles de mantener y actualizar.

## Implementación en Python

### a. Explicación paso a paso del código

#### Paso 1: Instalación de `drf-yasg`

Primero, necesitamos instalar la librería `drf-yasg`. Esto se puede hacer utilizando `pip`:

```bash
pip install drf-yasg
```

#### Paso 2: Configuración de `drf-yasg`

Una vez instalada la librería, debemos configurarla en nuestro proyecto Django. Para ello, editamos el archivo `settings.py` para incluir `drf_yasg` en la lista de aplicaciones instaladas:

```python
# settings.py

INSTALLED_APPS = [
    ...
    'rest_framework',
    'drf_yasg',
    ...
]
```

#### Paso 3: Generación de la documentación

A continuación, configuramos las rutas para la documentación en el archivo `urls.py`:

```python
# urls.py

from django.urls import path, re_path
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
    openapi.Info(
        title="API de Carrito de Compras",
        default_version='v1',
        description="Documentación de la API para el sistema de carrito de compras",
        terms_of_service="https://www.google.com/policies/terms/",
        contact=openapi.Contact(email="contacto@carritocompras.local"),
        license=openapi.License(name="BSD License"),
    ),
    public=True,
    permission_classes=(permissions.AllowAny,),
)

urlpatterns = [
    ...
    re_path(r'^swagger(?P<format>\.json|\.yaml)$', schema_view.without_ui(cache_timeout=0), name='schema-json'),
    path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    path('redoc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
    ...
]
```

#### Paso 4: Integración con vistas y endpoints

`drf-yasg` automáticamente genera la documentación basada en las vistas y serializers definidos en el proyecto. Asegúrate de que tus vistas y serializers estén correctamente configurados y documentados.

#### Paso 5: Pruebas y validación

Una vez configurada la documentación, accede a las URLs `/swagger/` y `/redoc/` en tu navegador para verificar que la documentación se genera correctamente y es precisa.

### b. Código fuente completo y comentado

```python
# settings.py

INSTALLED_APPS = [
    ...
    'rest_framework',
    'drf_yasg',
    ...
]

# urls.py

from django.urls import path, re_path
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

# Configuración de la vista del esquema
schema_view = get_schema_view(
    openapi.Info(
        title="API de Carrito de Compras",
        default_version='v1',
        description="Documentación de la API para el sistema de carrito de compras",
        terms_of_service="https://www.google.com/policies/terms/",
        contact=openapi.Contact(email="contacto@carritocompras.local"),
        license=openapi.License(name="BSD License"),
    ),
    public=True,
    permission_classes=(permissions.AllowAny,),
)

# Definición de las rutas para la documentación
urlpatterns = [
    ...
    re_path(r'^swagger(?P<format>\.json|\.yaml)$', schema_view.without_ui(cache_timeout=0), name='schema-json'),
    path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    path('redoc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
    ...
]
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Para acceder a la documentación de la API, abre tu navegador y navega a las siguientes URLs:

- Swagger UI: `http://localhost:8000/swagger/`
- Redoc: `http://localhost:8000/redoc/`

#### Pruebas unitarias

Podemos crear pruebas unitarias para asegurarnos de que las rutas de la documentación están funcionando correctamente.

```python
# tests.py

from django.test import TestCase
from django.urls import reverse

class DocumentationTests(TestCase):
    def test_swagger_ui(self):
        response = self.client.get(reverse('schema-swagger-ui'))
        self.assertEqual(response.status_code, 200)

    def test_redoc_ui(self):
        response = self.client.get(reverse('schema-redoc'))
        self.assertEqual(response.status_code, 200)
```

## Mejores prácticas y consideraciones de diseño

1. **Mantener la documentación actualizada**: Cada vez que se agreguen o modifiquen endpoints, asegúrate de que la documentación refleje estos cambios.
2. **Documentar parámetros y respuestas**: Proporciona descripciones claras y detalladas para los parámetros de entrada y las respuestas de cada endpoint.
3. **Seguridad**: Protege la documentación de la API para que solo usuarios autorizados puedan acceder a ella, especialmente en entornos de producción.
4. **Usabilidad**: Utiliza descripciones y ejemplos claros para facilitar la comprensión y el uso de la API.
5. **Automatización**: Considera la integración de la generación de documentación en el pipeline de CI/CD para asegurar que siempre esté actualizada.

### Consideraciones adicionales

- **Rendimiento**: La generación de la documentación no debe afectar el rendimiento de la API en producción. Considera el uso de caché para mejorar el rendimiento.
- **Escalabilidad**: A medida que la API crece, asegúrate de que la documentación siga siendo manejable y fácil de navegar.
- **Depuración**: Utiliza herramientas de depuración para identificar y solucionar problemas en la generación de la documentación.

Con estas prácticas y consideraciones, podrás mantener una documentación de API clara, precisa y útil, facilitando el trabajo de desarrolladores, testers y clientes que interactúan con tu sistema de carrito de compras.

# Unidad: Implementación de registro y login de usuarios

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, profundizaremos en la implementación de un sistema de autenticación y autorización para un sistema de carrito de compras utilizando Django. Aunque ya hemos cubierto los temas de registro y login de usuarios, en esta sección nos enfocaremos en aspectos más avanzados y específicos de la autenticación y autorización. Los objetivos de aprendizaje incluyen:

1. Comprender la importancia de la autenticación y autorización en aplicaciones web.
2. Implementar un sistema de roles y permisos para usuarios.
3. Configurar autenticación basada en tokens para APIs.
4. Integrar autenticación social (Google, Facebook).
5. Aplicar mejores prácticas de seguridad en la autenticación y autorización.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

El sistema de autenticación y autorización debe permitir a los usuarios registrarse, iniciar sesión, y acceder a diferentes funcionalidades del sistema de carrito de compras según sus roles y permisos. Además, debe soportar autenticación mediante tokens para APIs y permitir el inicio de sesión a través de proveedores de autenticación social como Google y Facebook.

### Casos de uso

1. **Registro de usuario**: Un nuevo usuario se registra proporcionando su correo electrónico, nombre de usuario y contraseña.
2. **Inicio de sesión**: Un usuario registrado inicia sesión proporcionando su nombre de usuario y contraseña.
3. **Roles y permisos**: Los usuarios tienen roles (administrador, cliente) que determinan su acceso a diferentes funcionalidades.
4. **Autenticación basada en tokens**: Usuarios autenticados pueden obtener un token para acceder a la API.
5. **Autenticación social**: Usuarios pueden iniciar sesión utilizando sus cuentas de Google o Facebook.

### Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Registro de Usuario]
    B --> C{Datos válidos?}
    C -->|Sí| D[Crear Usuario]
    C -->|No| E[Mostrar Error]
    D --> F[Inicio de Sesión]
    F --> G{Credenciales válidas?}
    G -->|Sí| H[Acceso al Sistema]
    G -->|No| I[Mostrar Error]
    H --> J[Asignar Roles y Permisos]
    J --> K[Acceso a Funcionalidades]
    H --> L[Obtener Token]
    L --> M[Acceso a API]
    H --> N[Autenticación Social]
    N --> O[Redirigir a Proveedor]
    O --> P[Validar Respuesta]
    P --> Q{Respuesta válida?}
    Q -->|Sí| R[Acceso al Sistema]
    Q -->|No| S[Mostrar Error]
```

### Requisitos no funcionales

1. **Seguridad**: Las contraseñas deben almacenarse de manera segura utilizando hashing. Las comunicaciones deben estar cifradas mediante HTTPS.
2. **Escalabilidad**: El sistema debe ser capaz de manejar un gran número de usuarios y solicitudes simultáneas.
3. **Rendimiento**: Las operaciones de autenticación deben ser rápidas y eficientes.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar.
5. **Compatibilidad**: El sistema debe ser compatible con diferentes navegadores y dispositivos.

## Implementación en Python

### Explicación paso a paso del código

En esta sección, implementaremos las funcionalidades descritas utilizando Django. Comenzaremos configurando el sistema de roles y permisos, luego implementaremos la autenticación basada en tokens y finalmente la autenticación social.

#### Configuración de roles y permisos

1. **Crear modelo de roles**: Definimos un modelo `Role` para almacenar los diferentes roles de usuario.

```python
from django.db import models
from django.contrib.auth.models import User

class Role(models.Model):
    name = models.CharField(max_length=50)
    users = models.ManyToManyField(User, related_name='roles')

    def __str__(self):
        return self.name
```

2. **Asignar roles a usuarios**: Creamos una vista para asignar roles a los usuarios.

```python
from django.shortcuts import render, get_object_or_404
from django.contrib.auth.models import User
from .models import Role

def assign_role(request, user_id, role_name):
    user = get_object_or_404(User, id=user_id)
    role = get_object_or_404(Role, name=role_name)
    user.roles.add(role)
    return render(request, 'assign_role.html', {'user': user, 'role': role})
```

3. **Configurar permisos**: Definimos permisos específicos para cada rol.

```python
from django.contrib.auth.decorators import user_passes_test

def role_required(role_name):
    def decorator(view_func):
        def _wrapped_view(request, *args, **kwargs):
            if request.user.roles.filter(name=role_name).exists():
                return view_func(request, *args, **kwargs)
            else:
                return render(request, 'access_denied.html')
        return _wrapped_view
    return decorator
```

#### Autenticación basada en tokens

1. **Instalar Django REST framework**: Instalamos el paquete necesario.

```bash
pip install djangorestframework
pip install djangorestframework-simplejwt
```

2. **Configurar Django REST framework**: Añadimos la configuración en `settings.py`.

```python
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
```

3. **Crear vistas para obtener tokens**: Definimos las vistas para manejar la autenticación basada en tokens.

```python
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView

urlpatterns = [
    ...
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
]
```

#### Autenticación social

1. **Instalar paquetes necesarios**: Instalamos los paquetes para autenticación social.

```bash
pip install social-auth-app-django
```

2. **Configurar autenticación social**: Añadimos la configuración en `settings.py`.

```python
INSTALLED_APPS = [
    ...
    'social_django',
]

AUTHENTICATION_BACKENDS = (
    'social_core.backends.google.GoogleOAuth2',
    'social_core.backends.facebook.FacebookOAuth2',
    'django.contrib.auth.backends.ModelBackend',
)

SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = '<your-client-id>'
SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = '<your-client-secret>'
SOCIAL_AUTH_FACEBOOK_KEY = '<your-app-id>'
SOCIAL_AUTH_FACEBOOK_SECRET = '<your-app-secret>'
```

3. **Configurar URLs y vistas**: Definimos las URLs y vistas para manejar la autenticación social.

```python
from django.urls import path, include

urlpatterns = [
    ...
    path('oauth/', include('social_django.urls', namespace='social')),
]
```

### Código fuente completo y comentado

```python
# models.py
from django.db import models
from django.contrib.auth.models import User

class Role(models.Model):
    name = models.CharField(max_length=50)
    users = models.ManyToManyField(User, related_name='roles')

    def __str__(self):
        return self.name

# views.py
from django.shortcuts import render, get_object_or_404
from django.contrib.auth.models import User
from .models import Role
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView

def assign_role(request, user_id, role_name):
    user = get_object_or_404(User, id=user_id)
    role = get_object_or_404(Role, name=role_name)
    user.roles.add(role)
    return render(request, 'assign_role.html', {'user': user, 'role': role})

# urls.py
from django.urls import path, include
from .views import assign_role

urlpatterns = [
    path('assign_role/<int:user_id>/<str:role_name>/', assign_role, name='assign_role'),
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
    path('oauth/', include('social_django.urls', namespace='social')),
]

# settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework_simplejwt',
    'social_django',
]

AUTHENTICATION_BACKENDS = (
    'social_core.backends.google.GoogleOAuth2',
    'social_core.backends.facebook.FacebookOAuth2',
    'django.contrib.auth.backends.ModelBackend',
)

SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = '<your-client-id>'
SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = '<your-client-secret>'
SOCIAL_AUTH_FACEBOOK_KEY = '<your-app-id>'
SOCIAL_AUTH_FACEBOOK_SECRET = '<your-app-secret>'

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplos de uso

1. **Asignar roles a usuarios**: Para asignar un rol a un usuario, podemos hacer una solicitud GET a la URL `/assign_role/<user_id>/<role_name>/`.

2. **Obtener token**: Para obtener un token, hacemos una solicitud POST a la URL `/api/token/` con el nombre de usuario y la contraseña.

3. **Autenticación social**: Para iniciar sesión con Google, redirigimos al usuario a la URL `/oauth/login/google-oauth2/`.

#### Pruebas unitarias

```python
from django.test import TestCase
from django.contrib.auth.models import User
from .models import Role

class RoleTestCase(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='testuser', password='12345')
        self.role = Role.objects.create(name='admin')

    def test_assign_role(self):
        self.user.roles.add(self.role)
        self.assertIn(self.role, self.user.roles.all())

    def test_token_authentication(self):
        response = self.client.post('/api/token/', {'username': 'testuser', 'password': '12345'})
        self.assertEqual(response.status_code, 200)
        self.assertIn('access', response.data)

    def test_social_authentication(self):
        # This test would require mocking the social authentication process
        pass
```

## Mejores prácticas y consideraciones de diseño

1. **Seguridad**: Asegúrate de utilizar HTTPS para todas las comunicaciones. Almacena las contraseñas de manera segura utilizando hashing (por ejemplo, con `bcrypt`).
2. **Roles y permisos**: Define claramente los roles y permisos necesarios para cada funcionalidad del sistema. Utiliza decoradores para verificar los permisos antes de acceder a las vistas.
3. **Autenticación basada en tokens**: Utiliza tokens JWT para la autenticación en APIs. Asegúrate de manejar correctamente la expiración y renovación de tokens.
4. **Autenticación social**: Facilita el inicio de sesión mediante proveedores de autenticación social. Asegúrate de manejar correctamente los tokens y la información del usuario.
5. **Pruebas**: Implementa pruebas unitarias y de integración para verificar que el sistema de autenticación y autorización funciona correctamente.
6. **Escalabilidad**: Diseña el sistema para manejar un gran número de usuarios y solicitudes simultáneas. Considera el uso de caché y balanceo de carga.
7. **Depuración**: Utiliza herramientas de depuración y logging para identificar y solucionar problemas rápidamente. Asegúrate de manejar correctamente los errores y excepciones.

Con esto, hemos cubierto de manera exhaustiva la implementación de un sistema de autenticación y autorización avanzado en Django, incluyendo roles y permisos, autenticación basada en tokens y autenticación social.

# Unidad: Uso de Django Allauth para autenticación social

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, profundizaremos en el uso de Django Allauth para la autenticación social dentro de un sistema de carrito de compras desarrollado con Django. Django Allauth es una biblioteca poderosa y versátil que simplifica la integración de múltiples proveedores de autenticación social, como Google, Facebook y Twitter, en aplicaciones Django. El objetivo de esta unidad es proporcionar una comprensión completa de cómo configurar y utilizar Django Allauth para permitir a los usuarios autenticarse mediante sus cuentas de redes sociales.

### Objetivos de aprendizaje:
1. Comprender los conceptos básicos de la autenticación social y su importancia en aplicaciones web.
2. Configurar Django Allauth en un proyecto Django.
3. Integrar múltiples proveedores de autenticación social utilizando Django Allauth.
4. Personalizar el flujo de autenticación y las vistas proporcionadas por Django Allauth.
5. Implementar pruebas unitarias para asegurar la funcionalidad correcta de la autenticación social.
6. Aplicar mejores prácticas y consideraciones de diseño para la autenticación social.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

La autenticación social permite a los usuarios registrarse e iniciar sesión en nuestra aplicación de carrito de compras utilizando sus cuentas de redes sociales. Esto no solo mejora la experiencia del usuario al reducir la fricción en el proceso de registro, sino que también aumenta la seguridad al delegar la autenticación a proveedores confiables. Django Allauth facilita la integración de múltiples proveedores de autenticación social, gestionando la complejidad de las diferentes APIs y flujos de autenticación.

### Casos de uso

1. **Registro e inicio de sesión con Google**:
   - **Actor**: Usuario
   - **Descripción**: Un usuario puede registrarse o iniciar sesión en la aplicación utilizando su cuenta de Google.
   - **Precondiciones**: El usuario debe tener una cuenta de Google.
   - **Flujo principal**:
     1. El usuario selecciona "Iniciar sesión con Google" en la página de inicio de sesión.
     2. El usuario es redirigido a la página de autenticación de Google.
     3. El usuario ingresa sus credenciales de Google y concede permisos a la aplicación.
     4. El usuario es redirigido de vuelta a la aplicación, donde se completa el proceso de autenticación.

2. **Registro e inicio de sesión con Facebook**:
   - **Actor**: Usuario
   - **Descripción**: Un usuario puede registrarse o iniciar sesión en la aplicación utilizando su cuenta de Facebook.
   - **Precondiciones**: El usuario debe tener una cuenta de Facebook.
   - **Flujo principal**:
     1. El usuario selecciona "Iniciar sesión con Facebook" en la página de inicio de sesión.
     2. El usuario es redirigido a la página de autenticación de Facebook.
     3. El usuario ingresa sus credenciales de Facebook y concede permisos a la aplicación.
     4. El usuario es redirigido de vuelta a la aplicación, donde se completa el proceso de autenticación.

### Diagramas de flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Seleccionar proveedor de autenticación]
    B --> C[Google]
    B --> D[Facebook]
    C --> E[Redirigir a Google]
    D --> F[Redirigir a Facebook]
    E --> G[Autenticación en Google]
    F --> H[Autenticación en Facebook]
    G --> I[Conceder permisos]
    H --> J[Conceder permisos]
    I --> K[Redirigir a la aplicación]
    J --> L[Redirigir a la aplicación]
    K --> M[Completar autenticación]
    L --> N[Completar autenticación]
    M --> O[Usuario autenticado]
    N --> O[Usuario autenticado]
```

### Requisitos no funcionales

1. **Seguridad**: La autenticación social debe ser segura y proteger los datos del usuario. Se deben utilizar conexiones HTTPS y seguir las mejores prácticas de seguridad recomendadas por los proveedores de autenticación.
2. **Rendimiento**: El proceso de autenticación debe ser rápido y eficiente, minimizando el tiempo de redirección y carga.
3. **Escalabilidad**: La solución debe ser capaz de manejar un gran número de usuarios simultáneos sin degradar el rendimiento.
4. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar, proporcionando una experiencia fluida para el usuario.

## Implementación en Python

### Explicación paso a paso del código

Para implementar la autenticación social utilizando Django Allauth, seguiremos estos pasos:

1. **Instalación de Django Allauth**:
   Primero, instalaremos Django Allauth y sus dependencias utilizando pip:
   ```bash
   pip install django-allauth
   ```

2. **Configuración de Django Allauth**:
   Luego, añadiremos `allauth` y sus aplicaciones relacionadas a nuestro archivo `settings.py`:
   ```python
   INSTALLED_APPS = [
       ...
       'django.contrib.sites',
       'allauth',
       'allauth.account',
       'allauth.socialaccount',
       'allauth.socialaccount.providers.google',
       'allauth.socialaccount.providers.facebook',
       ...
   ]

   SITE_ID = 1

   AUTHENTICATION_BACKENDS = (
       ...
       'allauth.account.auth_backends.AuthenticationBackend',
       ...
   )

   LOGIN_REDIRECT_URL = '/'
   ```

3. **Configuración de URLs**:
   Añadiremos las URLs de Allauth a nuestro archivo `urls.py`:
   ```python
   from django.urls import path, include

   urlpatterns = [
       ...
       path('accounts/', include('allauth.urls')),
       ...
   ]
   ```

4. **Configuración de proveedores de autenticación**:
   Configuraremos los proveedores de autenticación en el panel de administración de Django. Para Google, necesitaremos las credenciales de la API de Google. Para Facebook, necesitaremos una aplicación de Facebook configurada.

### Código fuente completo y comentado

A continuación, se presenta el código fuente completo y comentado para la configuración de Django Allauth:

#### settings.py

```python
import os
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/3.2/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'your-secret-key'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []

# Application definition
INSTALLED_APPS = [
    ...
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.google',
    'allauth.socialaccount.providers.facebook',
    ...
]

MIDDLEWARE = [
    ...
]

ROOT_URLCONF = 'myproject.urls'

TEMPLATES = [
    ...
]

WSGI_APPLICATION = 'myproject.wsgi.application'

# Database
# https://docs.djangoproject.com/en/3.2/ref/settings/#databases
DATABASES = {
    ...
}

# Password validation
# https://docs.djangoproject.com/en/3.2/ref/settings/#auth-password-validators
AUTH_PASSWORD_VALIDATORS = [
    ...
]

# Internationalization
# https://docs.djangoproject.com/en/3.2/topics/i18n/
LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

USE_TZ = True

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.2/howto/static-files/
STATIC_URL = '/static/'

# Default primary key field type
# https://docs.djangoproject.com/en/3.2/ref/settings/#default-auto-field
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

# Django Allauth configuration
SITE_ID = 1

AUTHENTICATION_BACKENDS = (
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
)

LOGIN_REDIRECT_URL = '/'

# Social account providers
SOCIALACCOUNT_PROVIDERS = {
    'google': {
        'SCOPE': [
            'profile',
            'email',
        ],
        'AUTH_PARAMS': {
            'access_type': 'online',
        }
    },
    'facebook': {
        'METHOD': 'oauth2',
        'SCOPE': ['email', 'public_profile'],
        'FIELDS': [
            'id',
            'email',
            'name',
            'first_name',
            'last_name',
            'verified',
            'locale',
            'timezone',
            'link',
            'gender',
            'updated_time',
        ],
        'EXCHANGE_TOKEN': True,
        'LOCALE_FUNC': lambda request: 'en_US',
        'VERIFIED_EMAIL': False,
        'VERSION': 'v7.0',
    }
}
```

#### urls.py

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls')),
    ...
]
```

### Ejemplos de uso y pruebas unitarias

Para asegurarnos de que la autenticación social funciona correctamente, implementaremos pruebas unitarias utilizando Django's `TestCase`.

#### tests.py

```python
from django.test import TestCase
from django.urls import reverse

class SocialAuthTests(TestCase):
    def test_google_login(self):
        response = self.client.get(reverse('socialaccount_login', args=['google']))
        self.assertEqual(response.status_code, 302)
        self.assertIn('accounts.google.com', response.url)

    def test_facebook_login(self):
        response = self.client.get(reverse('socialaccount_login', args=['facebook']))
        self.assertEqual(response.status_code, 302)
        self.assertIn('facebook.com', response.url)
```

### Mejores prácticas y consideraciones de diseño

1. **Seguridad**: Asegúrate de utilizar HTTPS en todas las comunicaciones entre el cliente y el servidor para proteger los datos del usuario. Configura correctamente las credenciales de la API y sigue las mejores prácticas de seguridad recomendadas por los proveedores de autenticación.
2. **Usabilidad**: Proporciona una interfaz de usuario intuitiva y fácil de usar para el proceso de autenticación social. Asegúrate de que los botones de inicio de sesión social sean claramente visibles y accesibles.
3. **Escalabilidad**: Diseña tu aplicación para manejar un gran número de usuarios simultáneos. Utiliza técnicas de caché y optimización de consultas para mejorar el rendimiento.
4. **Pruebas**: Implementa pruebas unitarias y de integración para asegurar que la autenticación social funcione correctamente en diferentes escenarios. Considera el uso de herramientas de prueba automatizadas para mejorar la cobertura de pruebas.
5. **Mantenimiento**: Mantén las dependencias de tu proyecto actualizadas y sigue las actualizaciones de seguridad y mejoras de Django Allauth y los proveedores de autenticación.

En conclusión, la integración de Django Allauth para la autenticación social en un sistema de carrito de compras no solo mejora la experiencia del usuario, sino que también proporciona una solución segura y escalable para la gestión de usuarios. Siguiendo los pasos y mejores prácticas descritos en esta unidad, podrás implementar una autenticación social robusta y eficiente en tu aplicación Django.

# Unidad: Personalización de Permisos de Django

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, exploraremos en profundidad la personalización de permisos en Django, una funcionalidad crucial para cualquier sistema de carrito de compras. Los permisos permiten controlar el acceso a diferentes partes de la aplicación, asegurando que solo los usuarios autorizados puedan realizar ciertas acciones. Al finalizar esta unidad, serás capaz de:

1. Comprender los conceptos básicos de permisos en Django.
2. Crear y gestionar permisos personalizados.
3. Implementar permisos a nivel de vista y modelo.
4. Utilizar decoradores y mixins para la autorización.
5. Integrar permisos personalizados en un sistema de carrito de compras.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

La personalización de permisos en Django permite definir reglas específicas sobre quién puede acceder a qué recursos y realizar qué acciones dentro de la aplicación. En el contexto de un sistema de carrito de compras, esto puede incluir permisos para agregar productos, modificar el carrito, realizar compras, y administrar el inventario.

### Casos de Uso

1. **Administrador del Sistema**: Puede agregar, modificar y eliminar productos del inventario, así como gestionar los pedidos de los clientes.
2. **Cliente Registrado**: Puede agregar productos al carrito, modificar la cantidad de productos en el carrito y realizar compras.
3. **Usuario Anónimo**: Puede navegar por los productos, pero no puede agregar productos al carrito ni realizar compras.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Usuario Anónimo]
    B -->|Navegar Productos| C[Navegar Productos]
    B -->|Intentar Agregar al Carrito| D[Redirigir a Login]
    A --> E[Usuario Registrado]
    E -->|Agregar al Carrito| F[Agregar Producto al Carrito]
    E -->|Modificar Carrito| G[Modificar Cantidad de Productos]
    E -->|Realizar Compra| H[Procesar Compra]
    A --> I[Administrador]
    I -->|Agregar Producto| J[Agregar Producto al Inventario]
    I -->|Modificar Producto| K[Modificar Producto en Inventario]
    I -->|Eliminar Producto| L[Eliminar Producto del Inventario]
    I -->|Gestionar Pedidos| M[Gestionar Pedidos de Clientes]
```

### Requisitos No Funcionales

1. **Seguridad**: Los permisos deben ser seguros y no deben permitir el acceso no autorizado a las funcionalidades críticas del sistema.
2. **Rendimiento**: La verificación de permisos debe ser eficiente y no debe afectar el rendimiento general de la aplicación.
3. **Escalabilidad**: El sistema de permisos debe ser escalable para manejar un gran número de usuarios y roles.
4. **Mantenibilidad**: El código relacionado con la gestión de permisos debe ser fácil de mantener y extender.

## Implementación en Python

### Explicación Paso a Paso del Código

#### Paso 1: Definir Permisos Personalizados

Primero, definiremos los permisos personalizados en nuestros modelos. Django permite agregar permisos personalizados utilizando la opción `permissions` en la clase `Meta` de un modelo.

```python
from django.db import models
from django.contrib.auth.models import User

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()

    class Meta:
        permissions = [
            ("puede_agregar_producto", "Puede agregar producto"),
            ("puede_modificar_producto", "Puede modificar producto"),
            ("puede_eliminar_producto", "Puede eliminar producto"),
        ]
```

#### Paso 2: Asignar Permisos a Usuarios y Grupos

Asignar permisos a usuarios y grupos es esencial para la gestión de roles. Esto se puede hacer en el archivo `admin.py` o mediante señales.

```python
from django.contrib import admin
from django.contrib.auth.models import Group, Permission
from django.contrib.contenttypes.models import ContentType
from .models import Producto

class ProductoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'precio', 'stock')

admin.site.register(Producto, ProductoAdmin)

# Asignar permisos a un grupo
content_type = ContentType.objects.get_for_model(Producto)
permiso_agregar = Permission.objects.get(
    codename='puede_agregar_producto',
    content_type=content_type,
)
permiso_modificar = Permission.objects.get(
    codename='puede_modificar_producto',
    content_type=content_type,
)
permiso_eliminar = Permission.objects.get(
    codename='puede_eliminar_producto',
    content_type=content_type,
)

grupo_administrador = Group.objects.get(name='Administrador')
grupo_administrador.permissions.add(permiso_agregar, permiso_modificar, permiso_eliminar)
```

#### Paso 3: Verificar Permisos en Vistas

Para verificar permisos en las vistas, utilizamos los decoradores `permission_required` o mixins como `PermissionRequiredMixin`.

```python
from django.contrib.auth.decorators import permission_required
from django.shortcuts import render, redirect
from .models import Producto

@permission_required('app.puede_agregar_producto', raise_exception=True)
def agregar_producto(request):
    if request.method == 'POST':
        nombre = request.POST['nombre']
        descripcion = request.POST['descripcion']
        precio = request.POST['precio']
        stock = request.POST['stock']
        Producto.objects.create(nombre=nombre, descripcion=descripcion, precio=precio, stock=stock)
        return redirect('lista_productos')
    return render(request, 'agregar_producto.html')
```

#### Paso 4: Implementar Permisos en el Sistema de Carrito de Compras

En el contexto de un carrito de compras, los permisos pueden ser utilizados para controlar el acceso a la funcionalidad de agregar productos al carrito, modificar el carrito y realizar compras.

```python
from django.contrib.auth.decorators import login_required
from django.contrib.auth.mixins import PermissionRequiredMixin
from django.views.generic import View
from django.shortcuts import render, redirect
from .models import Producto, Carrito

@login_required
def agregar_al_carrito(request, producto_id):
    producto = Producto.objects.get(id=producto_id)
    carrito, created = Carrito.objects.get_or_create(usuario=request.user, activo=True)
    carrito.productos.add(producto)
    return redirect('ver_carrito')

class ModificarCarritoView(PermissionRequiredMixin, View):
    permission_required = 'app.puede_modificar_carrito'

    def post(self, request, *args, **kwargs):
        producto_id = request.POST['producto_id']
        cantidad = request.POST['cantidad']
        carrito = Carrito.objects.get(usuario=request.user, activo=True)
        carrito.modificar_producto(producto_id, cantidad)
        return redirect('ver_carrito')
```

### Código Fuente Completo y Comentado

```python
# models.py
from django.db import models
from django.contrib.auth.models import User

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()

    class Meta:
        permissions = [
            ("puede_agregar_producto", "Puede agregar producto"),
            ("puede_modificar_producto", "Puede modificar producto"),
            ("puede_eliminar_producto", "Puede eliminar producto"),
        ]

class Carrito(models.Model):
    usuario = models.ForeignKey(User, on_delete=models.CASCADE)
    productos = models.ManyToManyField(Producto)
    activo = models.BooleanField(default=True)

    def modificar_producto(self, producto_id, cantidad):
        # Lógica para modificar la cantidad de un producto en el carrito
        pass

# admin.py
from django.contrib import admin
from django.contrib.auth.models import Group, Permission
from django.contrib.contenttypes.models import ContentType
from .models import Producto

class ProductoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'precio', 'stock')

admin.site.register(Producto, ProductoAdmin)

# Asignar permisos a un grupo
content_type = ContentType.objects.get_for_model(Producto)
permiso_agregar = Permission.objects.get(
    codename='puede_agregar_producto',
    content_type=content_type,
)
permiso_modificar = Permission.objects.get(
    codename='puede_modificar_producto',
    content_type=content_type,
)
permiso_eliminar = Permission.objects.get(
    codename='puede_eliminar_producto',
    content_type=content_type,
)

grupo_administrador = Group.objects.get(name='Administrador')
grupo_administrador.permissions.add(permiso_agregar, permiso_modificar, permiso_eliminar)

# views.py
from django.contrib.auth.decorators import permission_required, login_required
from django.contrib.auth.mixins import PermissionRequiredMixin
from django.views.generic import View
from django.shortcuts import render, redirect
from .models import Producto, Carrito

@permission_required('app.puede_agregar_producto', raise_exception=True)
def agregar_producto(request):
    if request.method == 'POST':
        nombre = request.POST['nombre']
        descripcion = request.POST['descripcion']
        precio = request.POST['precio']
        stock = request.POST['stock']
        Producto.objects.create(nombre=nombre, descripcion=descripcion, precio=precio, stock=stock)
        return redirect('lista_productos')
    return render(request, 'agregar_producto.html')

@login_required
def agregar_al_carrito(request, producto_id):
    producto = Producto.objects.get(id=producto_id)
    carrito, created = Carrito.objects.get_or_create(usuario=request.user, activo=True)
    carrito.productos.add(producto)
    return redirect('ver_carrito')

class ModificarCarritoView(PermissionRequiredMixin, View):
    permission_required = 'app.puede_modificar_carrito'

    def post(self, request, *args, **kwargs):
        producto_id = request.POST['producto_id']
        cantidad = request.POST['cantidad']
        carrito = Carrito.objects.get(usuario=request.user, activo=True)
        carrito.modificar_producto(producto_id, cantidad)
        return redirect('ver_carrito')
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

Para probar la funcionalidad de permisos personalizados, primero asegúrate de que los permisos estén asignados correctamente a los usuarios y grupos. Luego, intenta realizar acciones que requieran permisos específicos.

```python
# Crear un usuario y asignar permisos
from django.contrib.auth.models import User, Group

usuario = User.objects.create_user('usuario_test', 'test@example.com', 'password123')
grupo_administrador = Group.objects.get(name='Administrador')
usuario.groups.add(grupo_administrador)

# Intentar agregar un producto
from django.test import Client

client = Client()
client.login(username='usuario_test', password='password123')
response = client.post('/agregar_producto/', {'nombre': 'Producto Test', 'descripcion': 'Descripción del producto', 'precio': '100.00', 'stock': '10'})
assert response.status_code == 302  # Redirige a la lista de productos
```

#### Pruebas Unitarias

Las pruebas unitarias aseguran que los permisos funcionan como se espera.

```python
from django.test import TestCase
from django.contrib.auth.models import User, Group
from .models import Producto

class PermisosTestCase(TestCase):
    def setUp(self):
        self.usuario = User.objects.create_user('usuario_test', 'test@example.com', 'password123')
        self.grupo_administrador = Group.objects.create(name='Administrador')
        self.usuario.groups.add(self.grupo_administrador)
        self.producto = Producto.objects.create(nombre='Producto Test', descripcion='Descripción del producto', precio=100.00, stock=10)

    def test_agregar_producto(self):
        self.client.login(username='usuario_test', password='password123')
        response = self.client.post('/agregar_producto/', {'nombre': 'Nuevo Producto', 'descripcion': 'Nueva descripción', 'precio': '200.00', 'stock': '20'})
        self.assertEqual(response.status_code, 302)
        self.assertTrue(Producto.objects.filter(nombre='Nuevo Producto').exists())

    def test_modificar_producto(self):
        self.client.login(username='usuario_test', password='password123')
        response = self.client.post('/modificar_producto/', {'producto_id': self.producto.id, 'nombre': 'Producto Modificado', 'descripcion': 'Descripción modificada', 'precio': '150.00', 'stock': '15'})
        self.assertEqual(response.status_code, 302)
        self.producto.refresh_from_db()
        self.assertEqual(self.producto.nombre, 'Producto Modificado')
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Principio de Menor Privilegio**: Asigna solo los permisos necesarios a cada usuario o grupo para minimizar el riesgo de accesos no autorizados.
2. **Revisión Regular de Permisos**: Revisa y actualiza los permisos regularmente para asegurar que siguen siendo relevantes y seguros.
3. **Uso de Decoradores y Mixins**: Utiliza decoradores y mixins para implementar permisos de manera consistente y reutilizable.
4. **Pruebas Exhaustivas**: Implementa pruebas unitarias y de integración para asegurar que los permisos funcionan correctamente en todos los escenarios.
5. **Documentación**: Documenta claramente los permisos y su propósito para facilitar el mantenimiento y la extensión del sistema.

En resumen, la personalización de permisos en Django es una herramienta poderosa para controlar el acceso a diferentes partes de una aplicación. Al seguir las mejores prácticas y considerar aspectos de seguridad, rendimiento y escalabilidad, puedes crear un sistema robusto y seguro para tu aplicación de carrito de compras.

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

# Unidad: Protección de Vistas y API Endpoints

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en la protección de vistas y API endpoints dentro de un sistema de carrito de compras desarrollado con Django. Este tema es crucial para garantizar que solo usuarios autenticados y autorizados puedan acceder a ciertas funcionalidades y datos sensibles de la aplicación. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender la importancia de proteger vistas y API endpoints.
2. Implementar mecanismos de protección en vistas y API endpoints utilizando Django.
3. Configurar permisos específicos para diferentes tipos de usuarios.
4. Utilizar decoradores y mixins para simplificar la protección de vistas.
5. Realizar pruebas unitarias para verificar la seguridad de las vistas y API endpoints.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

La protección de vistas y API endpoints implica restringir el acceso a ciertas partes de la aplicación web basada en Django. Esto se logra mediante la implementación de mecanismos de autenticación y autorización que aseguran que solo los usuarios con los permisos adecuados puedan acceder a recursos específicos. En el contexto de un sistema de carrito de compras, esto puede incluir:

- Restringir el acceso a la vista del carrito de compras solo a usuarios autenticados.
- Permitir que solo los administradores puedan acceder a vistas de administración de productos.
- Proteger los endpoints de la API para que solo usuarios autenticados puedan realizar operaciones CRUD (Crear, Leer, Actualizar, Eliminar).

### Casos de Uso

1. **Acceso al Carrito de Compras**:
   - **Actor**: Usuario autenticado.
   - **Descripción**: Un usuario autenticado puede acceder a su carrito de compras.
   - **Precondición**: El usuario debe estar autenticado.
   - **Postcondición**: El usuario puede ver y modificar los productos en su carrito de compras.

2. **Administración de Productos**:
   - **Actor**: Administrador.
   - **Descripción**: Un administrador puede acceder a las vistas de administración de productos para agregar, editar o eliminar productos.
   - **Precondición**: El usuario debe tener permisos de administrador.
   - **Postcondición**: El administrador puede gestionar los productos en la tienda.

3. **Acceso a Endpoints de la API**:
   - **Actor**: Usuario autenticado.
   - **Descripción**: Un usuario autenticado puede realizar operaciones CRUD a través de los endpoints de la API.
   - **Precondición**: El usuario debe estar autenticado y tener los permisos necesarios.
   - **Postcondición**: El usuario puede interactuar con los recursos de la API según sus permisos.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B{¿Usuario autenticado?}
    B -- Sí --> C{¿Permisos adecuados?}
    B -- No --> D[Redirigir a página de login]
    C -- Sí --> E[Acceso concedido]
    C -- No --> F[Mostrar mensaje de error]
```

### Requisitos No Funcionales

1. **Seguridad**: La protección de vistas y API endpoints debe ser robusta para prevenir accesos no autorizados.
2. **Rendimiento**: La implementación de protección no debe afectar significativamente el rendimiento de la aplicación.
3. **Escalabilidad**: La solución debe ser escalable para soportar un gran número de usuarios y solicitudes.
4. **Mantenibilidad**: El código debe ser claro y fácil de mantener, permitiendo futuras modificaciones y mejoras.

## Implementación en Python

### Explicación Paso a Paso del Código

Para proteger vistas y API endpoints en Django, utilizaremos varias herramientas y técnicas, incluyendo decoradores, mixins y permisos personalizados.

1. **Protección de Vistas con Decoradores**:
   Django proporciona decoradores como `login_required` y `permission_required` que se pueden usar para proteger vistas basadas en funciones.

2. **Protección de Vistas con Mixins**:
   Para vistas basadas en clases, Django ofrece mixins como `LoginRequiredMixin` y `PermissionRequiredMixin`.

3. **Protección de Endpoints de la API**:
   Utilizaremos Django REST Framework (DRF) para proteger los endpoints de la API mediante permisos y autenticación basada en tokens.

### Código Fuente Completo y Comentado

#### Protección de Vistas Basadas en Funciones

```python
from django.contrib.auth.decorators import login_required, permission_required
from django.shortcuts import render, redirect

@login_required
def carrito_de_compras(request):
    # Lógica para mostrar el carrito de compras del usuario
    return render(request, 'carrito.html')

@permission_required('is_staff')
def administrar_productos(request):
    # Lógica para administrar productos
    return render(request, 'admin_productos.html')
```

#### Protección de Vistas Basadas en Clases

```python
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin
from django.views.generic import ListView, CreateView
from .models import Producto

class CarritoDeComprasView(LoginRequiredMixin, ListView):
    model = Producto
    template_name = 'carrito.html'

class AdministrarProductosView(PermissionRequiredMixin, ListView):
    permission_required = 'is_staff'
    model = Producto
    template_name = 'admin_productos.html'
```

#### Protección de Endpoints de la API

```python
from rest_framework.permissions import IsAuthenticated, IsAdminUser
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Producto
from .serializers import ProductoSerializer

class ProductoListCreate(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        productos = Producto.objects.all()
        serializer = ProductoSerializer(productos, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = ProductoSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

class ProductoAdmin(APIView):
    permission_classes = [IsAdminUser]

    def delete(self, request, pk):
        try:
            producto = Producto.objects.get(pk=pk)
            producto.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)
        except Producto.DoesNotExist:
            return Response(status=status.HTTP_404_NOT_FOUND)
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

Para probar la protección de vistas y endpoints, podemos crear usuarios con diferentes permisos y verificar el acceso a las vistas y endpoints.

```python
from django.test import TestCase
from django.urls import reverse
from django.contrib.auth.models import User

class ProteccionVistasTest(TestCase):

    def setUp(self):
        self.user = User.objects.create_user(username='usuario', password='password')
        self.admin = User.objects.create_superuser(username='admin', password='password')

    def test_acceso_carrito_sin_autenticar(self):
        response = self.client.get(reverse('carrito_de_compras'))
        self.assertRedirects(response, '/accounts/login/?next=/carrito/')

    def test_acceso_carrito_autenticado(self):
        self.client.login(username='usuario', password='password')
        response = self.client.get(reverse('carrito_de_compras'))
        self.assertEqual(response.status_code, 200)

    def test_acceso_admin_productos_sin_permisos(self):
        self.client.login(username='usuario', password='password')
        response = self.client.get(reverse('administrar_productos'))
        self.assertEqual(response.status_code, 403)

    def test_acceso_admin_productos_con_permisos(self):
        self.client.login(username='admin', password='password')
        response = self.client.get(reverse('administrar_productos'))
        self.assertEqual(response.status_code, 200)
```

### Mejores Prácticas y Consideraciones de Diseño

1. **Principio de Menor Privilegio**: Asigne solo los permisos necesarios a cada usuario para minimizar el riesgo de acceso no autorizado.
2. **Uso de Decoradores y Mixins**: Utilice decoradores y mixins para simplificar la protección de vistas y evitar la repetición de código.
3. **Pruebas de Seguridad**: Realice pruebas exhaustivas para asegurar que las vistas y endpoints están adecuadamente protegidos.
4. **Manejo de Errores**: Proporcione mensajes de error claros y seguros cuando un usuario no tenga acceso a una vista o endpoint.
5. **Monitoreo y Auditoría**: Implemente mecanismos de monitoreo y auditoría para detectar y responder a intentos de acceso no autorizado.

## Conclusión

La protección de vistas y API endpoints es un aspecto crítico en el desarrollo de aplicaciones web seguras. A través de esta unidad, hemos explorado diversas técnicas y herramientas que Django ofrece para implementar mecanismos de autenticación y autorización robustos. Al seguir las mejores prácticas y realizar pruebas exhaustivas, podemos asegurar que nuestra aplicación de carrito de compras es segura y confiable para todos los usuarios.

# Unidad: Integración con Pasarela de Pagos (e.g., Stripe)

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, profundizaremos en la integración de una pasarela de pagos, específicamente Stripe, en un sistema de carrito de compras desarrollado con Django. Stripe es una de las plataformas de pagos más populares y ofrece una API robusta y fácil de usar para procesar pagos en línea. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender los conceptos básicos de una pasarela de pagos.
2. Configurar y utilizar la API de Stripe en un proyecto Django.
3. Implementar un flujo de pago seguro y eficiente.
4. Realizar pruebas y manejar errores comunes en la integración de Stripe.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

La funcionalidad principal de esta unidad es integrar Stripe como pasarela de pagos en un sistema de carrito de compras desarrollado con Django. Esto incluye la configuración de Stripe, la creación de una interfaz de usuario para el pago, la gestión de transacciones y la actualización del estado del pedido una vez que el pago se haya completado.

### Casos de Uso

1. **Caso de Uso 1: Configuración de Stripe**
   - **Actor:** Administrador del sistema.
   - **Descripción:** El administrador configura las credenciales de Stripe en el sistema Django.
   - **Precondición:** El administrador debe tener una cuenta de Stripe y las credenciales de API.
   - **Postcondición:** Las credenciales de Stripe se almacenan de manera segura en el sistema.

2. **Caso de Uso 2: Proceso de Pago**
   - **Actor:** Usuario final.
   - **Descripción:** El usuario final selecciona productos, procede al checkout y realiza el pago utilizando Stripe.
   - **Precondición:** El usuario debe tener productos en su carrito y estar autenticado.
   - **Postcondición:** El pago se procesa exitosamente y el estado del pedido se actualiza.

3. **Caso de Uso 3: Manejo de Errores de Pago**
   - **Actor:** Usuario final.
   - **Descripción:** El sistema maneja errores de pago y proporciona retroalimentación al usuario.
   - **Precondición:** El usuario intenta realizar un pago.
   - **Postcondición:** El sistema muestra un mensaje de error adecuado y permite al usuario intentar nuevamente.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Usuario selecciona productos]
    B --> C[Usuario procede al checkout]
    C --> D[Usuario ingresa información de pago]
    D --> E[Stripe procesa el pago]
    E -->|Pago exitoso| F[Actualizar estado del pedido]
    E -->|Pago fallido| G[Mostrar mensaje de error]
    F --> H[Fin]
    G --> D
```

### Requisitos No Funcionales

1. **Seguridad:** La información de pago debe ser manejada de manera segura utilizando HTTPS y las mejores prácticas de seguridad.
2. **Rendimiento:** El proceso de pago debe ser rápido y eficiente, minimizando el tiempo de espera del usuario.
3. **Escalabilidad:** El sistema debe ser capaz de manejar un gran volumen de transacciones sin degradación del rendimiento.
4. **Usabilidad:** La interfaz de usuario debe ser intuitiva y fácil de usar, proporcionando una experiencia de pago fluida.

## Implementación en Python

### Explicación Paso a Paso del Código

#### Paso 1: Instalación y Configuración de Stripe

Primero, necesitamos instalar la biblioteca de Stripe para Python. Esto se puede hacer utilizando pip:

```bash
pip install stripe
```

Luego, configuramos las credenciales de Stripe en nuestro archivo `settings.py` de Django:

```python
# settings.py
STRIPE_SECRET_KEY = 'tu_clave_secreta_de_stripe'
STRIPE_PUBLISHABLE_KEY = 'tu_clave_publicable_de_stripe'
```

#### Paso 2: Creación del Formulario de Pago

Creamos un formulario de pago en Django utilizando `forms.py`:

```python
# forms.py
from django import forms

class PaymentForm(forms.Form):
    stripe_token = forms.CharField(widget=forms.HiddenInput())
```

#### Paso 3: Vista para Procesar el Pago

Creamos una vista en `views.py` para manejar el proceso de pago:

```python
# views.py
import stripe
from django.conf import settings
from django.shortcuts import render, redirect
from django.contrib import messages
from .forms import PaymentForm

stripe.api_key = settings.STRIPE_SECRET_KEY

def process_payment(request):
    if request.method == 'POST':
        form = PaymentForm(request.POST)
        if form.is_valid():
            stripe_token = form.cleaned_data['stripe_token']
            try:
                charge = stripe.Charge.create(
                    amount=5000,  # Monto en centavos
                    currency='usd',
                    description='Descripción del producto',
                    source=stripe_token
                )
                messages.success(request, 'Pago realizado con éxito')
                return redirect('success')
            except stripe.error.CardError as e:
                messages.error(request, f'Error en el pago: {e.error.message}')
                return redirect('checkout')
    else:
        form = PaymentForm()
    return render(request, 'checkout.html', {'form': form})
```

#### Paso 4: Plantilla de Checkout

Creamos una plantilla `checkout.html` para el formulario de pago:

```html
<!-- checkout.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Checkout</title>
    <script src="https://js.stripe.com/v3/"></script>
</head>
<body>
    <h1>Checkout</h1>
    <form action="{% url 'process_payment' %}" method="post" id="payment-form">
        {% csrf_token %}
        {{ form }}
        <button type="submit">Pagar</button>
    </form>
    <script>
        var stripe = Stripe('{{ stripe_publishable_key }}');
        var elements = stripe.elements();
        var card = elements.create('card');
        card.mount('#card-element');

        var form = document.getElementById('payment-form');
        form.addEventListener('submit', function(event) {
            event.preventDefault();
            stripe.createToken(card).then(function(result) {
                if (result.error) {
                    // Mostrar error en la interfaz de usuario
                } else {
                    var hiddenInput = document.createElement('input');
                    hiddenInput.setAttribute('type', 'hidden');
                    hiddenInput.setAttribute('name', 'stripe_token');
                    hiddenInput.setAttribute('value', result.token.id);
                    form.appendChild(hiddenInput);
                    form.submit();
                }
            });
        });
    </script>
</body>
</html>
```

### Código Fuente Completo y Comentado

```python
# settings.py
# Configuración de Stripe
STRIPE_SECRET_KEY = 'tu_clave_secreta_de_stripe'
STRIPE_PUBLISHABLE_KEY = 'tu_clave_publicable_de_stripe'

# forms.py
from django import forms

class PaymentForm(forms.Form):
    stripe_token = forms.CharField(widget=forms.HiddenInput())

# views.py
import stripe
from django.conf import settings
from django.shortcuts import render, redirect
from django.contrib import messages
from .forms import PaymentForm

stripe.api_key = settings.STRIPE_SECRET_KEY

def process_payment(request):
    if request.method == 'POST':
        form = PaymentForm(request.POST)
        if form.is_valid():
            stripe_token = form.cleaned_data['stripe_token']
            try:
                charge = stripe.Charge.create(
                    amount=5000,  # Monto en centavos
                    currency='usd',
                    description='Descripción del producto',
                    source=stripe_token
                )
                messages.success(request, 'Pago realizado con éxito')
                return redirect('success')
            except stripe.error.CardError as e:
                messages.error(request, f'Error en el pago: {e.error.message}')
                return redirect('checkout')
    else:
        form = PaymentForm()
    return render(request, 'checkout.html', {'form': form})

# checkout.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Checkout</title>
    <script src="https://js.stripe.com/v3/"></script>
</head>
<body>
    <h1>Checkout</h1>
    <form action="{% url 'process_payment' %}" method="post" id="payment-form">
        {% csrf_token %}
        {{ form }}
        <button type="submit">Pagar</button>
    </form>
    <script>
        var stripe = Stripe('{{ stripe_publishable_key }}');
        var elements = stripe.elements();
        var card = elements.create('card');
        card.mount('#card-element');

        var form = document.getElementById('payment-form');
        form.addEventListener('submit', function(event) {
            event.preventDefault();
            stripe.createToken(card).then(function(result) {
                if (result.error) {
                    // Mostrar error en la interfaz de usuario
                } else {
                    var hiddenInput = document.createElement('input');
                    hiddenInput.setAttribute('type', 'hidden');
                    hiddenInput.setAttribute('name', 'stripe_token');
                    hiddenInput.setAttribute('value', result.token.id);
                    form.appendChild(hiddenInput);
                    form.submit();
                }
            });
        });
    </script>
</body>
</html>
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

Para probar la integración, primero asegúrate de que el servidor de Django esté en ejecución. Luego, navega a la página de checkout, ingresa los detalles de la tarjeta de crédito y realiza un pago. Si todo está configurado correctamente, deberías ver un mensaje de éxito.

#### Pruebas Unitarias

Podemos escribir pruebas unitarias para verificar que la integración de Stripe funciona correctamente:

```python
# tests.py
from django.test import TestCase
from django.urls import reverse
from django.conf import settings
import stripe

class PaymentTestCase(TestCase):
    def setUp(self):
        stripe.api_key = settings.STRIPE_SECRET_KEY

    def test_payment_success(self):
        response = self.client.post(reverse('process_payment'), {
            'stripe_token': 'tok_visa',  # Token de prueba de Stripe
        })
        self.assertEqual(response.status_code, 302)
        self.assertRedirects(response, reverse('success'))

    def test_payment_failure(self):
        response = self.client.post(reverse('process_payment'), {
            'stripe_token': 'tok_chargeDeclined',  # Token de prueba de Stripe para fallo
        })
        self.assertEqual(response.status_code, 302)
        self.assertRedirects(response, reverse('checkout'))
```

## Mejores Prácticas y Consideraciones de Diseño

### Seguridad

1. **HTTPS:** Asegúrate de que tu sitio web esté utilizando HTTPS para proteger la información de pago.
2. **Manejo de Errores:** Implementa un manejo de errores robusto para capturar y manejar cualquier problema durante el proceso de pago.
3. **Validación de Datos:** Valida todos los datos de entrada para evitar ataques de inyección y otros tipos de vulnerabilidades.

### Rendimiento

1. **Optimización del Código:** Asegúrate de que el código esté optimizado para minimizar el tiempo de procesamiento.
2. **Carga Asíncrona:** Utiliza técnicas de carga asíncrona para mejorar la experiencia del usuario.

### Escalabilidad

1. **Balanceo de Carga:** Implementa balanceo de carga para manejar un gran volumen de transacciones.
2. **Monitoreo:** Utiliza herramientas de monitoreo para supervisar el rendimiento y la disponibilidad del sistema.

### Usabilidad

1. **Interfaz Intuitiva:** Diseña una interfaz de usuario intuitiva y fácil de usar.
2. **Retroalimentación al Usuario:** Proporciona retroalimentación clara y concisa al usuario durante el proceso de pago.

### Depuración y Solución de Problemas Comunes

1. **Logs:** Utiliza logs para registrar eventos importantes y errores.
2. **Pruebas:** Realiza pruebas exhaustivas para asegurar que el sistema funcione correctamente bajo diferentes condiciones.
3. **Documentación:** Mantén una documentación clara y actualizada sobre la configuración y el uso del sistema.

En conclusión, la integración de Stripe en un sistema de carrito de compras desarrollado con Django es un proceso que requiere atención a los detalles y una comprensión profunda de los conceptos de seguridad, rendimiento y usabilidad. Siguiendo las mejores prácticas y las consideraciones de diseño presentadas en esta unidad, podrás implementar una solución de pago robusta y eficiente que mejorará la experiencia del usuario y la seguridad de las transacciones.

# Unidad: Manejo de Webhooks para Actualizaciones de Pago

## 1. Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en el manejo de webhooks para actualizaciones de pago dentro del contexto de un sistema de carrito de compras desarrollado con Django. Los webhooks son una herramienta poderosa que permite a las aplicaciones recibir notificaciones en tiempo real sobre eventos específicos, como actualizaciones de pagos. Al final de esta unidad, los estudiantes serán capaces de:

1. Comprender el concepto y la importancia de los webhooks en la integración de pagos.
2. Configurar y manejar webhooks en Django.
3. Implementar lógica de negocio basada en eventos de pago recibidos a través de webhooks.
4. Asegurar y validar las solicitudes de webhook para evitar fraudes y errores.
5. Realizar pruebas y depuración de webhooks.

## 2. Documento Funcional de Requerimientos

### a. Descripción Detallada de la Funcionalidad

Los webhooks permiten a las aplicaciones recibir notificaciones automáticas de eventos importantes, como actualizaciones de pagos, directamente desde la pasarela de pagos (e.g., Stripe). En el contexto de nuestro sistema de carrito de compras, los webhooks se utilizarán para:

- Recibir notificaciones cuando un pago es exitoso.
- Actualizar el estado del pedido en la base de datos.
- Enviar confirmaciones de pedido al cliente.
- Manejar reembolsos y disputas de pagos.

### b. Casos de Uso

#### Caso de Uso 1: Notificación de Pago Exitoso

**Actor:** Pasarela de Pago (Stripe)

**Descripción:** Cuando un pago es exitoso, la pasarela de pago envía una notificación a nuestro sistema a través de un webhook. Nuestro sistema debe actualizar el estado del pedido a "Pagado" y enviar una confirmación al cliente.

**Flujo Principal:**

1. El cliente realiza un pago en la pasarela de pago.
2. La pasarela de pago envía un webhook a nuestro sistema.
3. Nuestro sistema valida la autenticidad del webhook.
4. Si el webhook es válido, se actualiza el estado del pedido a "Pagado".
5. Se envía una confirmación de pedido al cliente.

#### Caso de Uso 2: Manejo de Reembolsos

**Actor:** Pasarela de Pago (Stripe)

**Descripción:** Cuando se procesa un reembolso, la pasarela de pago envía una notificación a nuestro sistema a través de un webhook. Nuestro sistema debe actualizar el estado del pedido a "Reembolsado" y notificar al cliente.

**Flujo Principal:**

1. Un reembolso es procesado en la pasarela de pago.
2. La pasarela de pago envía un webhook a nuestro sistema.
3. Nuestro sistema valida la autenticidad del webhook.
4. Si el webhook es válido, se actualiza el estado del pedido a "Reembolsado".
5. Se envía una notificación de reembolso al cliente.

### c. Diagramas de Flujo

```mermaid
graph TD
    A[Cliente realiza un pago] --> B[Pasarela de Pago procesa el pago]
    B --> C[Pasarela de Pago envía webhook]
    C --> D[Servidor recibe el webhook]
    D --> E[Validar autenticidad del webhook]
    E --> F{Webhook válido?}
    F -->|Sí| G[Actualizar estado del pedido]
    G --> H[Enviar confirmación al cliente]
    F -->|No| I[Registrar error]
```

### d. Requisitos No Funcionales

1. **Seguridad:** Los webhooks deben ser validados para asegurar que provienen de una fuente confiable. Esto incluye la verificación de firmas y la autenticación.
2. **Rendimiento:** El manejo de webhooks debe ser eficiente para no afectar el rendimiento del sistema.
3. **Escalabilidad:** El sistema debe ser capaz de manejar un alto volumen de webhooks sin degradar su rendimiento.
4. **Disponibilidad:** El endpoint de los webhooks debe estar disponible en todo momento para recibir notificaciones en tiempo real.
5. **Registro y Monitoreo:** Todos los eventos de webhook deben ser registrados para auditoría y monitoreo.

## 3. Implementación en Python

### a. Explicación Paso a Paso del Código

Para implementar el manejo de webhooks en Django, seguiremos los siguientes pasos:

1. **Configuración del Endpoint de Webhook:** Crearemos una vista en Django para recibir las notificaciones de webhook.
2. **Validación del Webhook:** Implementaremos la lógica para validar la autenticidad del webhook.
3. **Procesamiento del Webhook:** Actualizaremos el estado del pedido y realizaremos las acciones necesarias basadas en el evento recibido.
4. **Notificación al Cliente:** Enviaremos confirmaciones o notificaciones al cliente según corresponda.

### b. Código Fuente Completo y Comentado

#### Configuración del Endpoint de Webhook

Primero, configuramos una URL en `urls.py` para recibir los webhooks:

```python
# urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('webhook/', views.webhook, name='webhook'),
]
```

#### Implementación de la Vista para Recibir Webhooks

En `views.py`, implementamos la vista para manejar los webhooks:

```python
# views.py
import json
import stripe
from django.conf import settings
from django.http import JsonResponse, HttpResponse
from django.views.decorators.csrf import csrf_exempt
from .models import Order

# Desactivar la protección CSRF para esta vista
@csrf_exempt
def webhook(request):
    payload = request.body
    sig_header = request.META['HTTP_STRIPE_SIGNATURE']
    event = None

    try:
        event = stripe.Webhook.construct_event(
            payload, sig_header, settings.STRIPE_WEBHOOK_SECRET
        )
    except ValueError as e:
        # Invalid payload
        return HttpResponse(status=400)
    except stripe.error.SignatureVerificationError as e:
        # Invalid signature
        return HttpResponse(status=400)

    # Handle the event
    if event['type'] == 'payment_intent.succeeded':
        payment_intent = event['data']['object']
        handle_payment_intent_succeeded(payment_intent)
    elif event['type'] == 'charge.refunded':
        charge = event['data']['object']
        handle_charge_refunded(charge)
    # ... handle other event types

    return JsonResponse({'status': 'success'})

def handle_payment_intent_succeeded(payment_intent):
    # Implementar la lógica para manejar el pago exitoso
    order_id = payment_intent['metadata']['order_id']
    order = Order.objects.get(id=order_id)
    order.status = 'Paid'
    order.save()
    # Enviar confirmación al cliente
    send_order_confirmation(order)

def handle_charge_refunded(charge):
    # Implementar la lógica para manejar el reembolso
    order_id = charge['metadata']['order_id']
    order = Order.objects.get(id=order_id)
    order.status = 'Refunded'
    order.save()
    # Notificar al cliente sobre el reembolso
    send_refund_notification(order)

def send_order_confirmation(order):
    # Implementar el envío de confirmación de pedido
    pass

def send_refund_notification(order):
    # Implementar el envío de notificación de reembolso
    pass
```

### c. Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

Para probar el webhook, podemos utilizar la herramienta de pruebas de Stripe para enviar eventos de prueba a nuestro endpoint:

```bash
stripe trigger payment_intent.succeeded
stripe trigger charge.refunded
```

#### Pruebas Unitarias

Implementamos pruebas unitarias para asegurar que nuestra lógica de manejo de webhooks funciona correctamente:

```python
# tests.py
from django.test import TestCase
from django.urls import reverse
from .models import Order

class WebhookTests(TestCase):
    def test_payment_intent_succeeded(self):
        order = Order.objects.create(id=1, status='Pending')
        payload = {
            'type': 'payment_intent.succeeded',
            'data': {
                'object': {
                    'metadata': {
                        'order_id': order.id
                    }
                }
            }
        }
        response = self.client.post(reverse('webhook'), data=json.dumps(payload), content_type='application/json')
        self.assertEqual(response.status_code, 200)
        order.refresh_from_db()
        self.assertEqual(order.status, 'Paid')

    def test_charge_refunded(self):
        order = Order.objects.create(id=1, status='Paid')
        payload = {
            'type': 'charge.refunded',
            'data': {
                'object': {
                    'metadata': {
                        'order_id': order.id
                    }
                }
            }
        }
        response = self.client.post(reverse('webhook'), data=json.dumps(payload), content_type='application/json')
        self.assertEqual(response.status_code, 200)
        order.refresh_from_db()
        self.assertEqual(order.status, 'Refunded')
```

## 4. Mejores Prácticas y Consideraciones de Diseño

### Seguridad

1. **Validación de Firmas:** Siempre validar la firma del webhook para asegurarse de que proviene de una fuente confiable.
2. **Protección CSRF:** Desactivar la protección CSRF solo para el endpoint de webhook.
3. **Registro y Monitoreo:** Registrar todos los eventos de webhook para auditoría y monitoreo.

### Rendimiento

1. **Asincronía:** Considerar el uso de tareas asincrónicas para manejar la lógica de negocio pesada, como el envío de correos electrónicos.
2. **Optimización de Consultas:** Optimizar las consultas a la base de datos para evitar cuellos de botella.

### Escalabilidad

1. **Colas de Mensajes:** Utilizar colas de mensajes para procesar webhooks de manera eficiente y escalable.
2. **Balanceo de Carga:** Implementar balanceo de carga para distribuir el tráfico de webhooks entre múltiples servidores.

### Depuración y Solución de Problemas

1. **Logs Detallados:** Mantener logs detallados de todos los eventos de webhook para facilitar la depuración.
2. **Herramientas de Prueba:** Utilizar herramientas de prueba proporcionadas por la pasarela de pago para simular eventos de webhook.
3. **Alertas:** Configurar alertas para detectar y responder rápidamente a cualquier problema con los webhooks.

Con estos conocimientos y prácticas, estarás bien equipado para manejar webhooks de manera efectiva en tu sistema de carrito de compras con Django.

### Unidad: Implementación de Proceso de Checkout

#### Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en la implementación del proceso de checkout dentro de un sistema de carrito de compras utilizando Django. El objetivo principal es proporcionar una guía detallada para desarrollar un flujo de checkout eficiente y seguro, que permita a los usuarios completar sus compras de manera intuitiva y sin fricciones. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender los componentes esenciales de un proceso de checkout.
2. Implementar un flujo de checkout en Django.
3. Integrar el proceso de checkout con una pasarela de pagos (e.g., Stripe).
4. Manejar errores y excepciones durante el proceso de checkout.
5. Aplicar mejores prácticas de seguridad y rendimiento en el flujo de checkout.

#### Documento Funcional de Requerimientos

##### Descripción Detallada de la Funcionalidad

El proceso de checkout es una secuencia de pasos que un usuario sigue para completar una compra en línea. Este proceso incluye la recopilación de información de envío, selección de métodos de pago, confirmación del pedido y finalización de la transacción. Los componentes clave de este flujo son:

1. **Página de Carrito de Compras**: Muestra los productos seleccionados por el usuario.
2. **Formulario de Información de Envío**: Recopila la dirección de envío y otros detalles relevantes.
3. **Selección de Método de Pago**: Permite al usuario elegir entre diferentes opciones de pago.
4. **Revisión del Pedido**: Muestra un resumen del pedido antes de la confirmación.
5. **Confirmación del Pedido**: Finaliza la transacción y muestra un mensaje de confirmación al usuario.

##### Casos de Uso

1. **Caso de Uso 1: Iniciar Proceso de Checkout**
   - **Actor**: Usuario
   - **Descripción**: El usuario inicia el proceso de checkout desde la página del carrito de compras.
   - **Precondiciones**: El usuario ha agregado al menos un producto al carrito.
   - **Flujo Principal**:
     1. El usuario hace clic en el botón "Checkout".
     2. El sistema redirige al formulario de información de envío.

2. **Caso de Uso 2: Completar Información de Envío**
   - **Actor**: Usuario
   - **Descripción**: El usuario completa el formulario de información de envío.
   - **Precondiciones**: El usuario ha iniciado el proceso de checkout.
   - **Flujo Principal**:
     1. El usuario ingresa la dirección de envío y otros detalles.
     2. El usuario hace clic en "Continuar".
     3. El sistema valida la información y redirige a la selección de método de pago.

3. **Caso de Uso 3: Seleccionar Método de Pago**
   - **Actor**: Usuario
   - **Descripción**: El usuario selecciona un método de pago.
   - **Precondiciones**: El usuario ha completado la información de envío.
   - **Flujo Principal**:
     1. El usuario elige un método de pago (e.g., tarjeta de crédito).
     2. El usuario hace clic en "Continuar".
     3. El sistema redirige a la revisión del pedido.

4. **Caso de Uso 4: Revisar y Confirmar Pedido**
   - **Actor**: Usuario
   - **Descripción**: El usuario revisa y confirma el pedido.
   - **Precondiciones**: El usuario ha seleccionado un método de pago.
   - **Flujo Principal**:
     1. El usuario revisa el resumen del pedido.
     2. El usuario hace clic en "Confirmar Pedido".
     3. El sistema procesa el pago y muestra una página de confirmación.

##### Diagramas de Flujo

```mermaid
graph TD
    A[Carrito de Compras] --> B[Iniciar Checkout]
    B --> C[Formulario de Información de Envío]
    C --> D[Seleccionar Método de Pago]
    D --> E[Revisión del Pedido]
    E --> F[Confirmar Pedido]
    F --> G[Página de Confirmación]
```

##### Requisitos No Funcionales

1. **Seguridad**: El proceso de checkout debe cumplir con los estándares de seguridad, incluyendo el uso de HTTPS y la protección de datos sensibles.
2. **Rendimiento**: El sistema debe ser capaz de manejar múltiples transacciones simultáneamente sin degradar el rendimiento.
3. **Usabilidad**: La interfaz de usuario debe ser intuitiva y fácil de usar, minimizando la fricción durante el proceso de checkout.
4. **Escalabilidad**: El sistema debe ser escalable para soportar un aumento en el volumen de transacciones.
5. **Disponibilidad**: El proceso de checkout debe estar disponible 24/7 con un tiempo de inactividad mínimo.

#### Implementación en Python

##### Explicación Paso a Paso del Código

Para implementar el proceso de checkout en Django, seguiremos estos pasos:

1. **Crear Modelos para la Información de Envío y Pedido**: Definiremos modelos para almacenar la información de envío y los detalles del pedido.
2. **Crear Formularios para la Información de Envío y Pago**: Utilizaremos formularios de Django para recopilar la información de envío y los detalles de pago.
3. **Desarrollar Vistas para Cada Paso del Checkout**: Implementaremos vistas basadas en clases para manejar cada paso del proceso de checkout.
4. **Integrar con la Pasarela de Pagos**: Utilizaremos la API de Stripe para procesar los pagos.
5. **Manejar Errores y Excepciones**: Implementaremos manejo de errores para asegurar una experiencia de usuario fluida.

##### Código Fuente Completo y Comentado

1. **Modelos**

```python
from django.db import models
from django.contrib.auth.models import User

class ShippingAddress(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    address = models.CharField(max_length=255)
    city = models.CharField(max_length=100)
    state = models.CharField(max_length=100)
    postal_code = models.CharField(max_length=20)
    country = models.CharField(max_length=100)

    def __str__(self):
        return f"{self.address}, {self.city}, {self.state}, {self.postal_code}, {self.country}"

class Order(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    shipping_address = models.ForeignKey(ShippingAddress, on_delete=models.SET_NULL, null=True)
    total_amount = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    is_paid = models.BooleanField(default=False)

    def __str__(self):
        return f"Order {self.id} by {self.user.username}"
```

2. **Formularios**

```python
from django import forms
from .models import ShippingAddress

class ShippingAddressForm(forms.ModelForm):
    class Meta:
        model = ShippingAddress
        fields = ['address', 'city', 'state', 'postal_code', 'country']

class PaymentForm(forms.Form):
    card_number = forms.CharField(max_length=16, required=True)
    expiry_date = forms.CharField(max_length=5, required=True)
    cvv = forms.CharField(max_length=3, required=True)
```

3. **Vistas**

```python
from django.shortcuts import render, redirect
from django.views import View
from .models import Order, ShippingAddress
from .forms import ShippingAddressForm, PaymentForm
import stripe

stripe.api_key = "tu_clave_secreta_de_stripe"

class CheckoutView(View):
    def get(self, request):
        form = ShippingAddressForm()
        return render(request, 'checkout/shipping.html', {'form': form})

    def post(self, request):
        form = ShippingAddressForm(request.POST)
        if form.is_valid():
            shipping_address = form.save(commit=False)
            shipping_address.user = request.user
            shipping_address.save()
            request.session['shipping_address_id'] = shipping_address.id
            return redirect('payment')
        return render(request, 'checkout/shipping.html', {'form': form})

class PaymentView(View):
    def get(self, request):
        form = PaymentForm()
        return render(request, 'checkout/payment.html', {'form': form})

    def post(self, request):
        form = PaymentForm(request.POST)
        if form.is_valid():
            shipping_address_id = request.session.get('shipping_address_id')
            shipping_address = ShippingAddress.objects.get(id=shipping_address_id)
            order = Order.objects.create(
                user=request.user,
                shipping_address=shipping_address,
                total_amount=100.00  # Este valor debe calcularse dinámicamente
            )
            try:
                charge = stripe.Charge.create(
                    amount=int(order.total_amount * 100),  # Stripe maneja centavos
                    currency='usd',
                    description=f'Order {order.id}',
                    source=request.POST['stripeToken']
                )
                order.is_paid = True
                order.save()
                return redirect('confirmation')
            except stripe.error.StripeError:
                order.delete()
                return render(request, 'checkout/payment.html', {'form': form, 'error': 'Error al procesar el pago'})
        return render(request, 'checkout/payment.html', {'form': form})

class ConfirmationView(View):
    def get(self, request):
        return render(request, 'checkout/confirmation.html')
```

4. **Plantillas**

- **shipping.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Información de Envío</title>
</head>
<body>
    <h1>Información de Envío</h1>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Continuar</button>
    </form>
</body>
</html>
```

- **payment.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Pago</title>
</head>
<body>
    <h1>Pago</h1>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Pagar</button>
    </form>
    {% if error %}
        <p style="color: red;">{{ error }}</p>
    {% endif %}
</body>
</html>
```

- **confirmation.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Confirmación de Pedido</title>
</head>
<body>
    <h1>¡Gracias por tu compra!</h1>
    <p>Tu pedido ha sido procesado con éxito.</p>
</body>
</html>
```

##### Ejemplos de Uso y Pruebas Unitarias

1. **Ejemplo de Uso**

Para probar el flujo de checkout, sigue estos pasos:

1. Inicia sesión como un usuario registrado.
2. Agrega productos al carrito de compras.
3. Navega a la página del carrito y haz clic en "Checkout".
4. Completa el formulario de información de envío y haz clic en "Continuar".
5. Completa el formulario de pago y haz clic en "Pagar".
6. Verifica que se redirige a la página de confirmación.

2. **Pruebas Unitarias**

```python
from django.test import TestCase
from django.urls import reverse
from django.contrib.auth.models import User
from .models import ShippingAddress, Order

class CheckoutTestCase(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='testuser', password='12345')
        self.client.login(username='testuser', password='12345')

    def test_shipping_address_form(self):
        response = self.client.post(reverse('checkout'), {
            'address': '123 Main St',
            'city': 'Springfield',
            'state': 'IL',
            'postal_code': '62701',
            'country': 'USA'
        })
        self.assertEqual(response.status_code, 302)
        self.assertTrue(ShippingAddress.objects.filter(user=self.user).exists())

    def test_payment_form(self):
        shipping_address = ShippingAddress.objects.create(
            user=self.user,
            address='123 Main St',
            city='Springfield',
            state='IL',
            postal_code='62701',
            country='USA'
        )
        self.client.session['shipping_address_id'] = shipping_address.id
        self.client.session.save()
        response = self.client.post(reverse('payment'), {
            'card_number': '4242424242424242',
            'expiry_date': '12/23',
            'cvv': '123',
            'stripeToken': 'tok_visa'
        })
        self.assertEqual(response.status_code, 302)
        self.assertTrue(Order.objects.filter(user=self.user, is_paid=True).exists())
```

#### Mejores Prácticas y Consideraciones de Diseño

1. **Seguridad**: Asegúrate de que todas las páginas del proceso de checkout utilicen HTTPS para proteger la información sensible del usuario. Además, valida y sanitiza todos los datos de entrada para prevenir ataques de inyección.

2. **Rendimiento**: Optimiza las consultas a la base de datos y utiliza técnicas de caché para mejorar el rendimiento del sistema. Considera el uso de colas de tareas para manejar procesos intensivos en segundo plano.

3. **Usabilidad**: Diseña una interfaz de usuario intuitiva y fácil de usar. Proporciona mensajes de error claros y útiles para guiar al usuario en caso de problemas durante el proceso de checkout.

4. **Escalabilidad**: Asegúrate de que el sistema sea escalable para manejar un aumento en el volumen de transacciones. Utiliza servicios de alojamiento escalables y considera la implementación de balanceadores de carga.

5. **Disponibilidad**: Implementa estrategias de alta disponibilidad para minimizar el tiempo de inactividad. Utiliza servicios de monitoreo para detectar y resolver problemas rápidamente.

6. **Depuración y Solución de Problemas**: Implementa un sistema de registro de errores para facilitar la depuración y solución de problemas. Proporciona una página de error amigable para el usuario en caso de fallos durante el proceso de checkout.

Al seguir estas mejores prácticas y consideraciones de diseño, podrás implementar un proceso de checkout eficiente, seguro y escalable en tu sistema de carrito de compras con Django.

# Unidad: Generación de Facturas con django-invoicing

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en la generación de facturas utilizando la librería `django-invoicing` dentro del contexto de un sistema de carrito de compras desarrollado con Django. Este tema es crucial para cualquier aplicación de comercio electrónico, ya que las facturas son documentos esenciales que registran las transacciones entre el vendedor y el comprador. Al finalizar esta unidad, los lectores estarán capacitados para:

1. Comprender la importancia de la generación de facturas en un sistema de comercio electrónico.
2. Configurar y utilizar la librería `django-invoicing` para generar facturas.
3. Integrar la generación de facturas en el flujo de trabajo del sistema de carrito de compras.
4. Implementar pruebas unitarias para asegurar la correcta generación de facturas.
5. Aplicar mejores prácticas y consideraciones de diseño en la generación de facturas.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

La funcionalidad de generación de facturas permitirá al sistema de carrito de compras crear y gestionar facturas para cada transacción completada. Las facturas deben incluir información detallada sobre los productos comprados, precios, impuestos, descuentos aplicados, y los datos del comprador y vendedor. La librería `django-invoicing` será utilizada para facilitar esta tarea.

### Casos de Uso

1. **Generación Automática de Facturas**: Al completar una transacción, el sistema debe generar automáticamente una factura y enviarla al correo electrónico del comprador.
2. **Visualización de Facturas**: Los usuarios deben poder visualizar y descargar sus facturas desde su cuenta.
3. **Administración de Facturas**: Los administradores deben poder acceder a todas las facturas generadas, filtrarlas por fecha, cliente, etc., y exportarlas en formato PDF o CSV.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Transacción Completada]
    B --> C[Generar Factura]
    C --> D[Guardar Factura en la Base de Datos]
    D --> E[Enviar Factura por Email]
    E --> F[Mostrar Factura en el Perfil del Usuario]
    F --> G[Fin]
```

### Requisitos No Funcionales

1. **Seguridad**: Las facturas deben ser accesibles solo por los usuarios autorizados. La información contenida en las facturas debe estar protegida.
2. **Rendimiento**: La generación de facturas debe ser rápida y no afectar el rendimiento general del sistema.
3. **Escalabilidad**: El sistema debe ser capaz de manejar un gran volumen de facturas sin degradar su rendimiento.
4. **Mantenibilidad**: El código debe ser claro y bien documentado para facilitar futuras modificaciones y mejoras.

## Implementación en Python

### Explicación Paso a Paso del Código

1. **Instalación de django-invoicing**:
   Para comenzar, necesitamos instalar la librería `django-invoicing`. Esto se puede hacer utilizando pip:
   ```bash
   pip install django-invoicing
   ```

2. **Configuración Inicial**:
   Agregamos `django-invoicing` a la lista de aplicaciones instaladas en `settings.py`:
   ```python
   INSTALLED_APPS = [
       ...
       'django_invoicing',
   ]
   ```

3. **Modelos de Factura**:
   Definimos los modelos necesarios para almacenar la información de las facturas. `django-invoicing` proporciona modelos predefinidos que podemos extender si es necesario.

4. **Generación de Facturas**:
   Implementamos la lógica para generar una factura cuando se completa una transacción. Esto puede hacerse en la vista o señal correspondiente.

5. **Envío de Facturas por Email**:
   Utilizamos el sistema de correo de Django para enviar la factura generada al correo electrónico del comprador.

6. **Visualización y Administración de Facturas**:
   Creamos vistas y plantillas para que los usuarios puedan ver y descargar sus facturas, y para que los administradores puedan gestionar todas las facturas.

### Código Fuente Completo y Comentado

```python
# models.py
from django.db import models
from django_invoicing.models import Invoice

class CustomInvoice(Invoice):
    # Podemos extender el modelo de Invoice si necesitamos campos adicionales
    pass

# signals.py
from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import Order, CustomInvoice

@receiver(post_save, sender=Order)
def generate_invoice(sender, instance, created, **kwargs):
    if created and instance.status == 'completed':
        invoice = CustomInvoice.objects.create(
            order=instance,
            customer=instance.customer,
            total_amount=instance.total_amount,
            # Otros campos necesarios
        )
        invoice.save()
        send_invoice_email(invoice)

# utils.py
from django.core.mail import send_mail
from django.template.loader import render_to_string
from .models import CustomInvoice

def send_invoice_email(invoice):
    subject = 'Su factura de compra'
    message = render_to_string('invoice_email.html', {'invoice': invoice})
    send_mail(subject, message, 'from@example.com', [invoice.customer.email])

# views.py
from django.shortcuts import render, get_object_or_404
from django.contrib.auth.decorators import login_required
from .models import CustomInvoice

@login_required
def view_invoice(request, invoice_id):
    invoice = get_object_or_404(CustomInvoice, id=invoice_id, customer=request.user)
    return render(request, 'invoice_detail.html', {'invoice': invoice})

# admin.py
from django.contrib import admin
from .models import CustomInvoice

@admin.register(CustomInvoice)
class CustomInvoiceAdmin(admin.ModelAdmin):
    list_display = ('id', 'customer', 'total_amount', 'created_at')
    search_fields = ('customer__name', 'id')
    list_filter = ('created_at',)
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

1. **Generación de Factura**:
   Cuando un usuario completa una compra, se genera automáticamente una factura y se envía por correo electrónico.

2. **Visualización de Factura**:
   El usuario puede acceder a su perfil y ver todas las facturas generadas para sus compras.

#### Pruebas Unitarias

```python
# tests.py
from django.test import TestCase
from django.contrib.auth.models import User
from .models import Order, CustomInvoice

class InvoiceGenerationTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='testuser', password='12345')
        self.order = Order.objects.create(customer=self.user, total_amount=100.0, status='completed')

    def test_invoice_creation(self):
        self.assertEqual(CustomInvoice.objects.count(), 1)
        invoice = CustomInvoice.objects.first()
        self.assertEqual(invoice.customer, self.user)
        self.assertEqual(invoice.total_amount, 100.0)

    def test_invoice_email(self):
        # Aquí podríamos simular el envío de correo y verificar que se envió correctamente
        pass
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Seguridad**:
   - Asegúrese de que solo los usuarios autenticados puedan acceder a sus facturas.
   - Utilice HTTPS para proteger la transmisión de datos sensibles.

2. **Rendimiento**:
   - Optimice las consultas a la base de datos para evitar cargas innecesarias.
   - Utilice caché para almacenar temporalmente las facturas generadas si es necesario.

3. **Escalabilidad**:
   - Diseñe el sistema para manejar un gran volumen de transacciones y facturas.
   - Considere el uso de servicios de almacenamiento externo para las facturas.

4. **Mantenibilidad**:
   - Mantenga el código bien documentado y organizado.
   - Escriba pruebas unitarias para asegurar la correcta funcionalidad del sistema.

5. **Depuración y Solución de Problemas**:
   - Utilice herramientas de logging para rastrear errores y problemas en la generación de facturas.
   - Realice pruebas exhaustivas en un entorno de desarrollo antes de desplegar cambios en producción.

Al seguir estas mejores prácticas y consideraciones de diseño, podrá implementar un sistema robusto y eficiente para la generación de facturas en su aplicación de carrito de compras con Django.

# Unidad: Manejo de errores y casos de borde en pagos

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en el manejo de errores y casos de borde en el proceso de pagos dentro de un sistema de carrito de compras desarrollado con Django. A lo largo de esta unidad, aprenderás a identificar y gestionar errores comunes que pueden ocurrir durante el proceso de pago, así como a manejar casos de borde que podrían afectar la experiencia del usuario y la integridad de los datos. Al finalizar esta unidad, estarás capacitado para:

1. Identificar y categorizar errores comunes en el proceso de pagos.
2. Implementar estrategias de manejo de errores en Django.
3. Gestionar casos de borde para garantizar la robustez del sistema.
4. Utilizar técnicas de depuración y pruebas unitarias para asegurar la calidad del código.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

El manejo de errores y casos de borde en pagos es crucial para garantizar una experiencia de usuario fluida y la integridad de los datos en un sistema de carrito de compras. Esta funcionalidad incluye la detección, registro y manejo de errores que pueden ocurrir durante el proceso de pago, así como la gestión de situaciones excepcionales que podrían comprometer la transacción.

#### Funcionalidades clave:

1. **Detección de errores en tiempo real**: Identificar errores durante el proceso de pago y proporcionar retroalimentación inmediata al usuario.
2. **Registro de errores**: Almacenar información detallada sobre los errores ocurridos para su análisis posterior.
3. **Manejo de casos de borde**: Gestionar situaciones excepcionales como pagos duplicados, transacciones fallidas, y problemas de conectividad.
4. **Notificaciones**: Informar al usuario y al administrador del sistema sobre errores críticos y casos de borde.

### Casos de uso

#### Caso de uso 1: Error de validación de tarjeta de crédito

**Descripción**: Un usuario intenta realizar un pago con una tarjeta de crédito inválida.

**Actor principal**: Usuario

**Flujo principal**:
1. El usuario ingresa los datos de su tarjeta de crédito en el formulario de pago.
2. El sistema valida los datos de la tarjeta.
3. Si los datos son inválidos, el sistema muestra un mensaje de error al usuario y solicita la corrección de los datos.

**Flujo alternativo**:
1. El usuario ingresa los datos de su tarjeta de crédito en el formulario de pago.
2. El sistema valida los datos de la tarjeta.
3. Si los datos son válidos, el sistema procede con la transacción.

#### Caso de uso 2: Problema de conectividad con la pasarela de pagos

**Descripción**: Durante el proceso de pago, se pierde la conexión con la pasarela de pagos.

**Actor principal**: Usuario

**Flujo principal**:
1. El usuario confirma el pago.
2. El sistema intenta comunicarse con la pasarela de pagos.
3. Si la conexión falla, el sistema muestra un mensaje de error al usuario y sugiere intentar nuevamente más tarde.

**Flujo alternativo**:
1. El usuario confirma el pago.
2. El sistema se comunica exitosamente con la pasarela de pagos.
3. La transacción se completa y el usuario recibe una confirmación.

### Diagramas de flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Usuario ingresa datos de tarjeta]
    B --> C{Datos válidos?}
    C -->|Sí| D[Procesar pago]
    C -->|No| E[Mostrar mensaje de error]
    D --> F{Conexión exitosa?}
    F -->|Sí| G[Confirmar transacción]
    F -->|No| H[Mostrar mensaje de error]
    G --> I[Fin]
    H --> I
    E --> B
```

### Requisitos no funcionales

1. **Rendimiento**: El sistema debe manejar errores y casos de borde sin afectar significativamente el tiempo de respuesta del proceso de pago.
2. **Escalabilidad**: El sistema debe ser capaz de manejar un aumento en la cantidad de transacciones y errores sin degradar su rendimiento.
3. **Seguridad**: Los datos de pago y los errores deben ser manejados de manera segura para proteger la información sensible del usuario.
4. **Mantenibilidad**: El código debe ser claro y modular para facilitar su mantenimiento y actualización.

## Implementación en Python

### Explicación paso a paso del código

Para implementar el manejo de errores y casos de borde en pagos, utilizaremos Django y algunas librerías adicionales como `requests` para manejar la comunicación con la pasarela de pagos. A continuación, se presenta una explicación detallada del código.

1. **Configuración del entorno**: Asegúrate de tener Django instalado y configurado. Además, instala la librería `requests` si aún no lo has hecho.

```bash
pip install django requests
```

2. **Modelo de errores**: Crea un modelo para registrar los errores en la base de datos.

```python
# models.py
from django.db import models

class PaymentError(models.Model):
    timestamp = models.DateTimeField(auto_now_add=True)
    error_message = models.TextField()
    user = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    transaction_id = models.CharField(max_length=100, null=True, blank=True)

    def __str__(self):
        return f"{self.timestamp} - {self.user} - {self.error_message}"
```

3. **Vista para manejar el pago**: Implementa una vista que maneje el proceso de pago y capture los errores.

```python
# views.py
from django.shortcuts import render, redirect
from django.contrib import messages
from .models import PaymentError
import requests

def process_payment(request):
    if request.method == 'POST':
        card_number = request.POST.get('card_number')
        expiry_date = request.POST.get('expiry_date')
        cvv = request.POST.get('cvv')
        amount = request.POST.get('amount')

        # Validar datos de la tarjeta
        if not validate_card(card_number, expiry_date, cvv):
            messages.error(request, "Datos de tarjeta inválidos.")
            return redirect('payment_page')

        try:
            # Intentar procesar el pago
            response = requests.post('https://api.pasarela-de-pagos.com/pagar', data={
                'card_number': card_number,
                'expiry_date': expiry_date,
                'cvv': cvv,
                'amount': amount
            })
            response.raise_for_status()
        except requests.exceptions.RequestException as e:
            # Registrar error en la base de datos
            PaymentError.objects.create(
                error_message=str(e),
                user=request.user
            )
            messages.error(request, "Hubo un problema con la pasarela de pagos. Por favor, intente nuevamente.")
            return redirect('payment_page')

        # Verificar respuesta de la pasarela de pagos
        if response.json().get('status') != 'success':
            PaymentError.objects.create(
                error_message=response.json().get('message'),
                user=request.user,
                transaction_id=response.json().get('transaction_id')
            )
            messages.error(request, "El pago no se pudo completar. Por favor, intente nuevamente.")
            return redirect('payment_page')

        # Pago exitoso
        messages.success(request, "Pago realizado con éxito.")
        return redirect('success_page')

    return render(request, 'payment_page.html')

def validate_card(card_number, expiry_date, cvv):
    # Implementar lógica de validación de tarjeta
    if len(card_number) != 16 or not card_number.isdigit():
        return False
    if len(cvv) != 3 or not cvv.isdigit():
        return False
    # Validar fecha de expiración (formato MM/AA)
    try:
        month, year = map(int, expiry_date.split('/'))
        if month < 1 or month > 12:
            return False
        if year < 22:  # Asumiendo que el año actual es 2022
            return False
    except ValueError:
        return False
    return True
```

### Código fuente completo y comentado

```python
# models.py
from django.db import models

class PaymentError(models.Model):
    timestamp = models.DateTimeField(auto_now_add=True)
    error_message = models.TextField()
    user = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    transaction_id = models.CharField(max_length=100, null=True, blank=True)

    def __str__(self):
        return f"{self.timestamp} - {self.user} - {self.error_message}"

# views.py
from django.shortcuts import render, redirect
from django.contrib import messages
from .models import PaymentError
import requests

def process_payment(request):
    if request.method == 'POST':
        card_number = request.POST.get('card_number')
        expiry_date = request.POST.get('expiry_date')
        cvv = request.POST.get('cvv')
        amount = request.POST.get('amount')

        # Validar datos de la tarjeta
        if not validate_card(card_number, expiry_date, cvv):
            messages.error(request, "Datos de tarjeta inválidos.")
            return redirect('payment_page')

        try:
            # Intentar procesar el pago
            response = requests.post('https://api.pasarela-de-pagos.com/pagar', data={
                'card_number': card_number,
                'expiry_date': expiry_date,
                'cvv': cvv,
                'amount': amount
            })
            response.raise_for_status()
        except requests.exceptions.RequestException as e:
            # Registrar error en la base de datos
            PaymentError.objects.create(
                error_message=str(e),
                user=request.user
            )
            messages.error(request, "Hubo un problema con la pasarela de pagos. Por favor, intente nuevamente.")
            return redirect('payment_page')

        # Verificar respuesta de la pasarela de pagos
        if response.json().get('status') != 'success':
            PaymentError.objects.create(
                error_message=response.json().get('message'),
                user=request.user,
                transaction_id=response.json().get('transaction_id')
            )
            messages.error(request, "El pago no se pudo completar. Por favor, intente nuevamente.")
            return redirect('payment_page')

        # Pago exitoso
        messages.success(request, "Pago realizado con éxito.")
        return redirect('success_page')

    return render(request, 'payment_page.html')

def validate_card(card_number, expiry_date, cvv):
    # Implementar lógica de validación de tarjeta
    if len(card_number) != 16 or not card_number.isdigit():
        return False
    if len(cvv) != 3 or not cvv.isdigit():
        return False
    # Validar fecha de expiración (formato MM/AA)
    try:
        month, year = map(int, expiry_date.split('/'))
        if month < 1 or month > 12:
            return False
        if year < 22:  # Asumiendo que el año actual es 2022
            return False
    except ValueError:
        return False
    return True
```

### Ejemplos de uso y pruebas unitarias

Para asegurarnos de que nuestra implementación es robusta, es fundamental realizar pruebas unitarias. Utilizaremos el framework de pruebas de Django para este propósito.

```python
# tests.py
from django.test import TestCase
from django.contrib.auth.models import User
from .models import PaymentError
from .views import validate_card

class PaymentTests(TestCase):

    def setUp(self):
        self.user = User.objects.create_user(username='testuser', password='12345')

    def test_validate_card(self):
        self.assertTrue(validate_card('1234567812345678', '12/23', '123'))
        self.assertFalse(validate_card('12345678', '12/23', '123'))
        self.assertFalse(validate_card('1234567812345678', '13/23', '123'))
        self.assertFalse(validate_card('1234567812345678', '12/21', '123'))
        self.assertFalse(validate_card('1234567812345678', '12/23', '12'))

    def test_process_payment_invalid_card(self):
        self.client.login(username='testuser', password='12345')
        response = self.client.post('/process_payment/', {
            'card_number': '12345678',
            'expiry_date': '12/23',
            'cvv': '123',
            'amount': '100'
        })
        self.assertEqual(response.status_code, 302)
        self.assertEqual(PaymentError.objects.count(), 0)

    def test_process_payment_valid_card(self):
        self.client.login(username='testuser', password='12345')
        with self.assertRaises(requests.exceptions.RequestException):
            self.client.post('/process_payment/', {
                'card_number': '1234567812345678',
                'expiry_date': '12/23',
                'cvv': '123',
                'amount': '100'
            })
        self.assertEqual(PaymentError.objects.count(), 1)
```

## Mejores prácticas y consideraciones de diseño

1. **Registro exhaustivo de errores**: Asegúrate de registrar todos los errores relevantes, incluyendo detalles como el usuario afectado y el contexto de la transacción. Esto facilitará la depuración y el análisis posterior.

2. **Retroalimentación al usuario**: Proporciona mensajes de error claros y útiles al usuario. Evita mensajes técnicos y enfócate en instrucciones que el usuario pueda seguir para corregir el problema.

3. **Pruebas unitarias y de integración**: Implementa pruebas unitarias y de integración para asegurar que el manejo de errores y casos de borde funcione correctamente. Esto incluye pruebas para diferentes escenarios de error y casos de borde.

4. **Seguridad**: Asegúrate de que los datos de pago y los errores se manejen de manera segura. Evita registrar información sensible como números de tarjeta de crédito en los registros de errores.

5. **Mantenibilidad**: Mantén el código modular y bien documentado. Esto facilitará su mantenimiento y actualización en el futuro.

6. **Monitoreo y alertas**: Implementa un sistema de monitoreo y alertas para detectar y responder rápidamente a errores críticos y casos de borde. Esto puede incluir el uso de herramientas de monitoreo de aplicaciones y servicios de notificación.

En resumen, el manejo de errores y casos de borde en pagos es una parte esencial de cualquier sistema de carrito de compras. Al seguir las mejores prácticas y utilizar un enfoque sistemático para la detección, registro y manejo de errores, puedes garantizar una experiencia de usuario fluida y la integridad de los datos en tu aplicación.

# Unidad: Implementación de Caché con Django's Cache Framework

## 1. Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, exploraremos en profundidad cómo implementar y optimizar el uso de caché en un sistema de carrito de compras desarrollado con Django. La caché es una técnica esencial para mejorar el rendimiento y la escalabilidad de aplicaciones web, permitiendo almacenar temporalmente datos frecuentemente solicitados para reducir la carga en el servidor y acelerar los tiempos de respuesta.

### Objetivos de Aprendizaje

Al finalizar esta unidad, los lectores serán capaces de:
1. Comprender los conceptos básicos y avanzados del uso de caché en Django.
2. Implementar diferentes estrategias de caché en un sistema de carrito de compras.
3. Configurar y utilizar el framework de caché de Django para optimizar el rendimiento.
4. Realizar pruebas y validaciones para asegurar la correcta implementación de la caché.
5. Aplicar mejores prácticas y consideraciones de diseño para el uso eficiente de la caché.

## 2. Documento Funcional de Requerimientos

### a. Descripción Detallada de la Funcionalidad

La funcionalidad de caché en un sistema de carrito de compras tiene como objetivo principal mejorar el rendimiento del sistema, reduciendo el tiempo de carga de las páginas y la carga en el servidor. La implementación de caché en Django se puede realizar en varios niveles, incluyendo caché de vistas, caché de fragmentos y caché de datos.

**Caché de Vistas:** Almacena el resultado de una vista completa, de modo que las solicitudes posteriores pueden ser servidas directamente desde la caché.

**Caché de Fragmentos:** Permite almacenar partes específicas de una página, como el contenido de un carrito de compras, para evitar la regeneración completa de la página.

**Caché de Datos:** Almacena datos específicos, como resultados de consultas a la base de datos, para reducir el tiempo de acceso a estos datos.

### b. Casos de Uso

1. **Caché de Vistas para la Página Principal del Carrito:**
   - **Actor:** Usuario
   - **Descripción:** Cuando un usuario accede a la página principal del carrito de compras, el sistema debe servir la página desde la caché si está disponible, reduciendo el tiempo de carga.
   - **Precondiciones:** La página debe haber sido visitada previamente y almacenada en la caché.
   - **Postcondiciones:** El usuario ve la página del carrito de compras rápidamente.

2. **Caché de Fragmentos para el Contenido del Carrito:**
   - **Actor:** Usuario
   - **Descripción:** Al agregar o eliminar productos del carrito, solo el fragmento del carrito debe actualizarse en la caché, mientras que el resto de la página permanece sin cambios.
   - **Precondiciones:** El usuario debe estar interactuando con el carrito de compras.
   - **Postcondiciones:** El contenido del carrito se actualiza rápidamente sin recargar toda la página.

3. **Caché de Datos para Consultas de Productos:**
   - **Actor:** Sistema
   - **Descripción:** Las consultas frecuentes a la base de datos para obtener información de productos deben ser almacenadas en la caché para reducir el tiempo de acceso.
   - **Precondiciones:** Los datos de productos deben ser solicitados frecuentemente.
   - **Postcondiciones:** Las consultas a la base de datos se reducen, mejorando el rendimiento del sistema.

### c. Diagramas de Flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Solicitud de Página del Carrito]
    B --> C{Página en Caché?}
    C -->|Sí| D[Servir Página desde Caché]
    C -->|No| E[Generar Página]
    E --> F[Almacenar Página en Caché]
    F --> D
    D --> G[Fin]

    subgraph Actualización del Carrito
        H[Agregar/Eliminar Producto] --> I[Actualizar Fragmento del Carrito]
        I --> J[Actualizar Caché del Fragmento]
        J --> G
    end
```

### d. Requisitos No Funcionales

1. **Rendimiento:** La implementación de la caché debe mejorar significativamente los tiempos de respuesta de las páginas del carrito de compras.
2. **Escalabilidad:** El sistema debe ser capaz de manejar un aumento en el número de usuarios concurrentes sin degradar el rendimiento.
3. **Consistencia:** La caché debe ser actualizada adecuadamente para reflejar los cambios en los datos del carrito de compras.
4. **Seguridad:** La caché no debe almacenar información sensible sin las debidas medidas de seguridad.
5. **Mantenibilidad:** El código relacionado con la caché debe ser fácil de entender y mantener.

## 3. Implementación en Python

### a. Explicación Paso a Paso del Código

Para implementar la caché en nuestro sistema de carrito de compras con Django, seguiremos los siguientes pasos:

1. **Configuración de la Caché en Django:**
   - Django soporta varios backends de caché, como Memcached, Redis y caché en memoria local. Configuraremos el backend de caché en el archivo `settings.py`.

2. **Implementación de Caché de Vistas:**
   - Utilizaremos los decoradores de caché de Django para almacenar el resultado de vistas completas.

3. **Implementación de Caché de Fragmentos:**
   - Utilizaremos la plantilla de caché de Django para almacenar partes específicas de una página.

4. **Implementación de Caché de Datos:**
   - Utilizaremos la API de caché de Django para almacenar y recuperar datos específicos.

### b. Código Fuente Completo y Comentado

#### Configuración de la Caché en `settings.py`

```python
# settings.py

# Configuración del backend de caché
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
        'LOCATION': '127.0.0.1:11211',
    }
}
```

#### Implementación de Caché de Vistas

```python
# views.py
from django.views.decorators.cache import cache_page
from django.shortcuts import render

@cache_page(60 * 15)  # Cachear la vista por 15 minutos
def carrito_view(request):
    # Lógica para obtener los datos del carrito
    productos = obtener_productos_del_carrito(request)
    return render(request, 'carrito.html', {'productos': productos})
```

#### Implementación de Caché de Fragmentos

```html
<!-- carrito.html -->
{% load cache %}
{% cache 600 carrito_fragmento %}
<div id="carrito">
    {% for producto in productos %}
        <div class="producto">
            <h2>{{ producto.nombre }}</h2>
            <p>{{ producto.precio }}</p>
        </div>
    {% endfor %}
</div>
{% endcache %}
```

#### Implementación de Caché de Datos

```python
# utils.py
from django.core.cache import cache

def obtener_productos():
    productos = cache.get('productos')
    if not productos:
        productos = Producto.objects.all()
        cache.set('productos', productos, 60 * 15)  # Cachear por 15 minutos
    return productos
```

### c. Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

```python
# views.py
from django.shortcuts import render
from .utils import obtener_productos

def productos_view(request):
    productos = obtener_productos()
    return render(request, 'productos.html', {'productos': productos})
```

#### Pruebas Unitarias

```python
# tests.py
from django.test import TestCase
from django.core.cache import cache
from .models import Producto

class CacheTests(TestCase):
    def setUp(self):
        Producto.objects.create(nombre="Producto 1", precio=100)
        Producto.objects.create(nombre="Producto 2", precio=200)

    def test_obtener_productos(self):
        productos = obtener_productos()
        self.assertEqual(len(productos), 2)
        cache.clear()
        productos = obtener_productos()
        self.assertEqual(len(productos), 2)
```

## 4. Mejores Prácticas y Consideraciones de Diseño

### Mejores Prácticas

1. **Elegir el Backend de Caché Adecuado:** Seleccionar el backend de caché que mejor se adapte a las necesidades de la aplicación. Memcached y Redis son opciones populares para aplicaciones de alta concurrencia.
2. **Configurar el Tiempo de Expiración:** Establecer tiempos de expiración adecuados para los datos en caché para asegurar que la información no se vuelva obsoleta.
3. **Invalidación de Caché:** Implementar mecanismos para invalidar la caché cuando los datos subyacentes cambian, asegurando la consistencia de la información.
4. **Monitoreo y Análisis:** Utilizar herramientas de monitoreo para analizar el rendimiento de la caché y ajustar la configuración según sea necesario.

### Consideraciones de Diseño

1. **Consistencia de Datos:** Asegurar que los datos en caché sean consistentes con los datos en la base de datos, especialmente en aplicaciones que manejan transacciones.
2. **Seguridad:** No almacenar información sensible en la caché sin las debidas medidas de seguridad, como cifrado.
3. **Escalabilidad:** Diseñar la caché para que pueda escalar horizontalmente, distribuyendo la carga entre múltiples servidores de caché.
4. **Mantenibilidad:** Escribir código de caché que sea fácil de entender y mantener, documentando claramente las estrategias de caché utilizadas.

### Consejos para la Depuración y Solución de Problemas Comunes

1. **Verificar la Configuración del Backend:** Asegurarse de que el backend de caché esté correctamente configurado y en funcionamiento.
2. **Revisar los Tiempos de Expiración:** Ajustar los tiempos de expiración si los datos en caché se vuelven obsoletos demasiado rápido o permanecen en caché por demasiado tiempo.
3. **Monitorear el Uso de la Caché:** Utilizar herramientas de monitoreo para identificar cuellos de botella y ajustar la configuración de la caché en consecuencia.
4. **Probar la Invalidación de Caché:** Asegurarse de que los mecanismos de invalidación de caché funcionen correctamente para mantener la consistencia de los datos.

Con esta unidad, hemos cubierto en detalle cómo implementar y optimizar el uso de caché en un sistema de carrito de compras con Django. Al seguir estas prácticas y consideraciones, los desarrolladores pueden mejorar significativamente el rendimiento y la escalabilidad de sus aplicaciones.

# Unidad: Optimización de consultas con `select_related` y `prefetch_related`

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la optimización de consultas en Django utilizando las técnicas `select_related` y `prefetch_related`. Estas herramientas son fundamentales para mejorar el rendimiento de una aplicación web, especialmente en sistemas complejos como un carrito de compras. Al finalizar esta unidad, serás capaz de:

1. Comprender la diferencia entre `select_related` y `prefetch_related`.
2. Implementar estas técnicas en un proyecto Django.
3. Evaluar el impacto de estas optimizaciones en el rendimiento de la aplicación.
4. Aplicar mejores prácticas para el uso de `select_related` y `prefetch_related`.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

El objetivo principal de esta unidad es optimizar las consultas a la base de datos en un sistema de carrito de compras utilizando `select_related` y `prefetch_related`. Estas técnicas permiten reducir el número de consultas a la base de datos, mejorando así el tiempo de respuesta de la aplicación.

### Casos de uso

1. **Caso de uso 1: Visualización de productos en el carrito**
   - **Descripción**: El usuario agrega varios productos al carrito y desea visualizar los detalles de cada producto.
   - **Requerimientos**: La aplicación debe cargar los detalles de los productos de manera eficiente, minimizando el número de consultas a la base de datos.

2. **Caso de uso 2: Listado de órdenes del usuario**
   - **Descripción**: El usuario desea ver un historial de sus órdenes, incluyendo los detalles de cada producto en cada orden.
   - **Requerimientos**: La aplicación debe cargar los detalles de las órdenes y los productos asociados de manera eficiente.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Consulta inicial a la base de datos]
    B --> C{¿Usar select_related?}
    C -->|Sí| D[Realizar consulta con select_related]
    C -->|No| E{¿Usar prefetch_related?}
    E -->|Sí| F[Realizar consulta con prefetch_related]
    E -->|No| G[Realizar consulta sin optimización]
    D --> H[Procesar resultados]
    F --> H[Procesar resultados]
    G --> H[Procesar resultados]
    H --> I[Mostrar resultados al usuario]
    I --> J[Fin]
```

### Requisitos no funcionales

1. **Rendimiento**: Las consultas optimizadas deben reducir el tiempo de respuesta en al menos un 30% en comparación con las consultas no optimizadas.
2. **Escalabilidad**: La solución debe ser escalable para manejar un aumento en el número de usuarios y productos sin degradar significativamente el rendimiento.
3. **Mantenibilidad**: El código debe ser claro y fácil de mantener, con comentarios adecuados y siguiendo las mejores prácticas de Django.

## Implementación en Python

### Explicación paso a paso del código

Para ilustrar el uso de `select_related` y `prefetch_related`, utilizaremos un ejemplo de un sistema de carrito de compras con dos modelos principales: `Order` y `Product`.

#### Modelos

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=255)
    price = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.name

class Order(models.Model):
    user = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    products = models.ManyToManyField(Product, through='OrderProduct')
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"Order {self.id} by {self.user.username}"

class OrderProduct(models.Model):
    order = models.ForeignKey(Order, on_delete=models.CASCADE)
    product = models.ForeignKey(Product, on_delete=models.CASCADE)
    quantity = models.PositiveIntegerField()

    def __str__(self):
        return f"{self.quantity} x {self.product.name} in order {self.order.id}"
```

#### Uso de `select_related`

`select_related` se utiliza para realizar una unión SQL y obtener los datos relacionados en una sola consulta. Es útil para relaciones `ForeignKey` y `OneToOne`.

```python
# Consultar órdenes y usuarios usando select_related
orders = Order.objects.select_related('user').all()

for order in orders:
    print(f"Order ID: {order.id}, User: {order.user.username}")
```

#### Uso de `prefetch_related`

`prefetch_related` se utiliza para realizar múltiples consultas y luego hacer la unión en Python. Es útil para relaciones `ManyToMany` y `ForeignKey` inversas.

```python
# Consultar órdenes y productos usando prefetch_related
orders = Order.objects.prefetch_related('products').all()

for order in orders:
    print(f"Order ID: {order.id}")
    for product in order.products.all():
        print(f"Product: {product.name}, Price: {product.price}")
```

### Código fuente completo y comentado

```python
from django.db import models

# Definición de los modelos
class Product(models.Model):
    name = models.CharField(max_length=255)
    price = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.name

class Order(models.Model):
    user = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    products = models.ManyToManyField(Product, through='OrderProduct')
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"Order {self.id} by {self.user.username}"

class OrderProduct(models.Model):
    order = models.ForeignKey(Order, on_delete=models.CASCADE)
    product = models.ForeignKey(Product, on_delete=models.CASCADE)
    quantity = models.PositiveIntegerField()

    def __str__(self):
        return f"{self.quantity} x {self.product.name} in order {self.order.id}"

# Uso de select_related para optimizar consultas de órdenes y usuarios
orders = Order.objects.select_related('user').all()

for order in orders:
    print(f"Order ID: {order.id}, User: {order.user.username}")

# Uso de prefetch_related para optimizar consultas de órdenes y productos
orders = Order.objects.prefetch_related('products').all()

for order in orders:
    print(f"Order ID: {order.id}")
    for product in order.products.all():
        print(f"Product: {product.name}, Price: {product.price}")
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

```python
# Crear algunos productos
product1 = Product.objects.create(name="Laptop", price=1000.00)
product2 = Product.objects.create(name="Mouse", price=50.00)

# Crear una orden y agregar productos
order = Order.objects.create(user=some_user)
OrderProduct.objects.create(order=order, product=product1, quantity=1)
OrderProduct.objects.create(order=order, product=product2, quantity=2)

# Consultar y mostrar órdenes con select_related y prefetch_related
orders = Order.objects.select_related('user').prefetch_related('products').all()

for order in orders:
    print(f"Order ID: {order.id}, User: {order.user.username}")
    for product in order.products.all():
        print(f"Product: {product.name}, Price: {product.price}")
```

#### Pruebas unitarias

```python
from django.test import TestCase
from django.contrib.auth.models import User

class OrderTestCase(TestCase):
    def setUp(self):
        self.user = User.objects.create(username='testuser')
        self.product1 = Product.objects.create(name="Laptop", price=1000.00)
        self.product2 = Product.objects.create(name="Mouse", price=50.00)
        self.order = Order.objects.create(user=self.user)
        OrderProduct.objects.create(order=self.order, product=self.product1, quantity=1)
        OrderProduct.objects.create(order=self.order, product=self.product2, quantity=2)

    def test_select_related(self):
        orders = Order.objects.select_related('user').all()
        for order in orders:
            self.assertEqual(order.user.username, 'testuser')

    def test_prefetch_related(self):
        orders = Order.objects.prefetch_related('products').all()
        for order in orders:
            products = list(order.products.all())
            self.assertEqual(len(products), 2)
            self.assertIn(self.product1, products)
            self.assertIn(self.product2, products)
```

## Mejores prácticas y consideraciones de diseño

1. **Usar `select_related` para relaciones `ForeignKey` y `OneToOne`**: Esta técnica es más eficiente para estas relaciones ya que realiza una sola consulta SQL con una unión.
2. **Usar `prefetch_related` para relaciones `ManyToMany` y `ForeignKey` inversas**: Esta técnica es más eficiente para estas relaciones ya que realiza múltiples consultas y luego hace la unión en Python.
3. **Evaluar el impacto en el rendimiento**: Siempre mide el impacto de estas optimizaciones en el rendimiento de tu aplicación. Utiliza herramientas como Django Debug Toolbar para analizar las consultas SQL generadas.
4. **Considerar la escalabilidad**: A medida que tu aplicación crece, es importante reevaluar tus optimizaciones y ajustarlas según sea necesario.
5. **Mantener el código limpio y comentado**: Asegúrate de que tu código sea fácil de entender y mantener. Incluye comentarios que expliquen por qué se están utilizando `select_related` y `prefetch_related`.

### Consejos para la depuración y solución de problemas comunes

1. **Verificar las consultas SQL generadas**: Utiliza Django Debug Toolbar para verificar las consultas SQL generadas y asegurarte de que `select_related` y `prefetch_related` están funcionando como se espera.
2. **Probar con datos reales**: Realiza pruebas con datos reales para asegurarte de que las optimizaciones funcionan en un entorno de producción.
3. **Revisar la documentación de Django**: La documentación oficial de Django es una excelente fuente de información y ejemplos sobre cómo usar `select_related` y `prefetch_related`.

Con estas técnicas y mejores prácticas, estarás bien equipado para optimizar las consultas en tu sistema de carrito de compras, mejorando así el rendimiento y la experiencia del usuario.

# Unidad: Uso de índices de base de datos

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, exploraremos en detalle el uso de índices de base de datos en el contexto de un sistema de carrito de compras desarrollado con Django. Los índices son una herramienta crucial para mejorar el rendimiento de las consultas en bases de datos, especialmente cuando se manejan grandes volúmenes de datos. Al finalizar esta unidad, los estudiantes serán capaces de:

1. Comprender qué son los índices de base de datos y cómo funcionan.
2. Identificar cuándo y dónde utilizar índices en una base de datos.
3. Implementar índices en Django utilizando modelos y migraciones.
4. Evaluar el impacto de los índices en el rendimiento de las consultas.
5. Aplicar mejores prácticas para el uso de índices en bases de datos.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

Los índices de base de datos son estructuras que mejoran la velocidad de las operaciones de consulta en tablas de bases de datos. Un índice se crea en una columna o un conjunto de columnas de una tabla, y permite a la base de datos encontrar rápidamente las filas que coinciden con los criterios de búsqueda.

En el contexto de un sistema de carrito de compras, los índices pueden ser utilizados para optimizar consultas frecuentes, como la búsqueda de productos, la recuperación de detalles de pedidos y la gestión de usuarios. Implementar índices de manera efectiva puede reducir significativamente el tiempo de respuesta de estas operaciones, mejorando la experiencia del usuario.

### b. Casos de uso

#### Caso de uso 1: Búsqueda de productos

**Descripción**: Un usuario busca productos en la tienda en línea utilizando palabras clave.

**Requerimientos**:
- Crear un índice en la columna `nombre` de la tabla `Producto` para acelerar las búsquedas por nombre.
- Crear un índice en la columna `categoria` de la tabla `Producto` para acelerar las búsquedas por categoría.

#### Caso de uso 2: Recuperación de detalles de pedidos

**Descripción**: Un usuario consulta los detalles de un pedido específico.

**Requerimientos**:
- Crear un índice en la columna `usuario_id` de la tabla `Pedido` para acelerar la recuperación de pedidos por usuario.
- Crear un índice en la columna `fecha_creacion` de la tabla `Pedido` para acelerar la recuperación de pedidos por fecha.

#### Caso de uso 3: Gestión de usuarios

**Descripción**: Un administrador gestiona la información de los usuarios registrados.

**Requerimientos**:
- Crear un índice en la columna `email` de la tabla `Usuario` para acelerar las búsquedas por correo electrónico.
- Crear un índice en la columna `fecha_registro` de la tabla `Usuario` para acelerar la recuperación de usuarios por fecha de registro.

### c. Diagramas de flujo (en mermaid)

```mermaid
flowchart TD
    A[Inicio] --> B[Identificar columnas frecuentemente consultadas]
    B --> C[Crear índices en las columnas identificadas]
    C --> D[Evaluar el rendimiento de las consultas]
    D --> E{¿Mejora significativa?}
    E -->|Sí| F[Implementar índices en producción]
    E -->|No| G[Revisar y ajustar índices]
    F --> H[Fin]
    G --> B
    H --> I[Monitorear el rendimiento]
```

### d. Requisitos no funcionales

1. **Rendimiento**: Los índices deben mejorar significativamente el tiempo de respuesta de las consultas sin afectar negativamente el rendimiento de las operaciones de inserción, actualización y eliminación.
2. **Escalabilidad**: La solución debe ser escalable y capaz de manejar un aumento en el volumen de datos sin degradar el rendimiento.
3. **Seguridad**: La implementación de índices no debe comprometer la seguridad de los datos.
4. **Mantenibilidad**: El código y las configuraciones relacionadas con los índices deben ser fáciles de mantener y actualizar.

## Implementación en Python

### a. Explicación paso a paso del código

Para implementar índices en Django, utilizaremos los modelos y las migraciones. Django proporciona varias opciones para definir índices en los modelos, incluyendo índices únicos, índices compuestos y más.

### b. Código fuente completo y comentado

A continuación, se presenta el código fuente completo y comentado para implementar los índices en el sistema de carrito de compras.

```python
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=255)
    categoria = models.CharField(max_length=255)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    descripcion = models.TextField()

    class Meta:
        indexes = [
            models.Index(fields=['nombre'], name='idx_producto_nombre'),
            models.Index(fields=['categoria'], name='idx_producto_categoria'),
        ]

class Pedido(models.Model):
    usuario = models.ForeignKey('Usuario', on_delete=models.CASCADE)
    fecha_creacion = models.DateTimeField(auto_now_add=True)
    total = models.DecimalField(max_digits=10, decimal_places=2)

    class Meta:
        indexes = [
            models.Index(fields=['usuario'], name='idx_pedido_usuario'),
            models.Index(fields=['fecha_creacion'], name='idx_pedido_fecha_creacion'),
        ]

class Usuario(models.Model):
    nombre = models.CharField(max_length=255)
    email = models.EmailField(unique=True)
    fecha_registro = models.DateTimeField(auto_now_add=True)

    class Meta:
        indexes = [
            models.Index(fields=['email'], name='idx_usuario_email'),
            models.Index(fields=['fecha_registro'], name='idx_usuario_fecha_registro'),
        ]
```

### c. Ejemplos de uso y pruebas unitarias

Para verificar que los índices se han creado correctamente y están mejorando el rendimiento de las consultas, podemos utilizar las siguientes pruebas unitarias.

```python
from django.test import TestCase
from .models import Producto, Pedido, Usuario

class IndicesTestCase(TestCase):
    def setUp(self):
        # Crear usuarios de prueba
        self.usuario = Usuario.objects.create(nombre='Juan Perez', email='juan@example.com')

        # Crear productos de prueba
        for i in range(1000):
            Producto.objects.create(nombre=f'Producto {i}', categoria='Categoria 1', precio=100.0, descripcion='Descripción del producto')

        # Crear pedidos de prueba
        for i in range(100):
            Pedido.objects.create(usuario=self.usuario, total=1000.0)

    def test_busqueda_producto_por_nombre(self):
        with self.assertNumQueries(1):
            productos = Producto.objects.filter(nombre__icontains='Producto 1')
            self.assertTrue(productos.exists())

    def test_recuperacion_pedidos_por_usuario(self):
        with self.assertNumQueries(1):
            pedidos = Pedido.objects.filter(usuario=self.usuario)
            self.assertTrue(pedidos.exists())

    def test_busqueda_usuario_por_email(self):
        with self.assertNumQueries(1):
            usuario = Usuario.objects.get(email='juan@example.com')
            self.assertIsNotNone(usuario)
```

## Mejores prácticas y consideraciones de diseño

1. **Evaluar el impacto de los índices**: Antes de implementar un índice, es importante evaluar su impacto en el rendimiento de las consultas y en las operaciones de escritura. Utilizar herramientas de monitoreo y análisis de rendimiento para tomar decisiones informadas.

2. **Evitar índices redundantes**: Crear índices innecesarios puede aumentar el tiempo de inserción y actualización de datos. Asegúrate de que cada índice tenga un propósito claro y mejore el rendimiento de consultas específicas.

3. **Índices compuestos**: En algunos casos, puede ser beneficioso crear índices compuestos en múltiples columnas. Por ejemplo, un índice en las columnas `usuario_id` y `fecha_creacion` de la tabla `Pedido` puede acelerar consultas que filtran por ambos campos.

4. **Mantener los índices actualizados**: A medida que cambian los patrones de uso y las consultas, es posible que los índices existentes necesiten ser ajustados o eliminados. Realiza revisiones periódicas del rendimiento de los índices y ajusta según sea necesario.

5. **Consideraciones de seguridad**: Asegúrate de que los índices no expongan información sensible. Por ejemplo, evita crear índices en columnas que contengan datos sensibles como contraseñas o información personal identificable.

6. **Documentación y mantenimiento**: Mantén una documentación clara de los índices creados y su propósito. Esto facilitará el mantenimiento y la actualización de la base de datos en el futuro.

7. **Pruebas y monitoreo**: Realiza pruebas exhaustivas para asegurarte de que los índices están mejorando el rendimiento como se espera. Utiliza herramientas de monitoreo para rastrear el impacto de los índices en el rendimiento de la base de datos a lo largo del tiempo.

### Consideraciones adicionales

- **Depuración**: Si experimentas problemas de rendimiento después de crear índices, utiliza herramientas de depuración y análisis de consultas para identificar y resolver los problemas.
- **Escalabilidad**: A medida que la base de datos crece, es posible que necesites ajustar los índices para mantener un rendimiento óptimo. Considera el uso de particionamiento de tablas y otras técnicas de escalabilidad.
- **Rendimiento**: Los índices pueden mejorar significativamente el rendimiento de las consultas, pero también pueden aumentar el tiempo de inserción y actualización de datos. Encuentra un equilibrio adecuado entre el rendimiento de las consultas y las operaciones de escritura.

Al seguir estas mejores prácticas y consideraciones de diseño, podrás implementar índices de manera efectiva en tu sistema de carrito de compras con Django, mejorando el rendimiento y la experiencia del usuario.

---

Esta unidad proporciona una guía exhaustiva para el uso de índices de base de datos en un sistema de carrito de compras desarrollado con Django. Al aplicar los conceptos y técnicas presentados, los desarrolladores podrán optimizar el rendimiento de sus aplicaciones y ofrecer una mejor experiencia a los usuarios.

# Unidad: Paginación de Resultados en Listados

## 1. Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en la paginación de resultados en listados dentro del contexto de un sistema de carrito de compras desarrollado con Django. La paginación es una técnica esencial para mejorar la experiencia del usuario y optimizar el rendimiento del sistema al dividir grandes conjuntos de datos en páginas más manejables. Al final de esta unidad, los estudiantes serán capaces de:

1. Comprender la importancia de la paginación en aplicaciones web.
2. Implementar la paginación en Django utilizando las herramientas y métodos proporcionados por el framework.
3. Diseñar interfaces de usuario que soporten la navegación paginada.
4. Optimizar la paginación para mejorar el rendimiento y la escalabilidad.
5. Realizar pruebas unitarias y de integración para asegurar la correcta implementación de la paginación.

## 2. Documento Funcional de Requerimientos

### a. Descripción Detallada de la Funcionalidad

La paginación de resultados en listados es una funcionalidad que permite a los usuarios navegar a través de grandes conjuntos de datos de manera eficiente. En un sistema de carrito de compras, esto puede aplicarse a listados de productos, órdenes, historial de compras, entre otros. La paginación divide el conjunto de datos en páginas más pequeñas, cada una de las cuales contiene un número fijo de elementos. Los usuarios pueden navegar entre estas páginas utilizando controles de navegación.

### b. Casos de Uso

#### Caso de Uso 1: Visualización de Productos

**Actor:** Usuario

**Descripción:** El usuario navega a la página de productos y visualiza una lista paginada de productos disponibles.

**Precondiciones:** El usuario debe estar en la página de productos.

**Postcondiciones:** El usuario puede navegar entre las páginas de productos utilizando los controles de navegación.

**Flujo Principal:**
1. El usuario accede a la página de productos.
2. El sistema muestra la primera página de productos.
3. El usuario utiliza los controles de navegación para moverse a la siguiente o anterior página.
4. El sistema actualiza la lista de productos según la página seleccionada.

#### Caso de Uso 2: Historial de Compras

**Actor:** Usuario registrado

**Descripción:** El usuario registrado accede a su historial de compras y visualiza una lista paginada de sus órdenes anteriores.

**Precondiciones:** El usuario debe estar autenticado y tener órdenes en su historial.

**Postcondiciones:** El usuario puede navegar entre las páginas de su historial de compras utilizando los controles de navegación.

**Flujo Principal:**
1. El usuario accede a la sección de historial de compras.
2. El sistema muestra la primera página del historial de compras.
3. El usuario utiliza los controles de navegación para moverse a la siguiente o anterior página.
4. El sistema actualiza la lista de órdenes según la página seleccionada.

### c. Diagramas de Flujo

```mermaid
graph TD
    A[Usuario accede a la página de productos] --> B[El sistema muestra la primera página de productos]
    B --> C{¿El usuario navega a otra página?}
    C -- Sí --> D[El sistema actualiza la lista de productos]
    D --> C
    C -- No --> E[Fin del flujo]
```

```mermaid
graph TD
    A[Usuario accede a la sección de historial de compras] --> B[El sistema muestra la primera página del historial]
    B --> C{¿El usuario navega a otra página?}
    C -- Sí --> D[El sistema actualiza la lista de órdenes]
    D --> C
    C -- No --> E[Fin del flujo]
```

### d. Requisitos No Funcionales

1. **Rendimiento:** La paginación debe ser eficiente y no debe causar una carga significativa en el servidor.
2. **Usabilidad:** Los controles de navegación deben ser intuitivos y fáciles de usar.
3. **Escalabilidad:** La implementación debe ser capaz de manejar un gran número de elementos sin degradar el rendimiento.
4. **Seguridad:** La paginación no debe exponer datos sensibles ni permitir la manipulación de parámetros para acceder a datos no autorizados.

## 3. Implementación en Python

### a. Explicación Paso a Paso del Código

Para implementar la paginación en Django, utilizaremos la clase `Paginator` proporcionada por el framework. A continuación, se detalla el proceso paso a paso:

1. **Importar la clase Paginator:**
   Primero, importamos la clase `Paginator` y otras utilidades necesarias desde `django.core.paginator`.

2. **Crear una vista para listar los productos:**
   En la vista, obtendremos todos los productos y los pasaremos a la clase `Paginator` para dividirlos en páginas.

3. **Obtener la página solicitada:**
   Utilizaremos el parámetro GET `page` para determinar qué página mostrar al usuario.

4. **Renderizar la plantilla con los productos paginados:**
   Pasaremos los objetos paginados a la plantilla para su visualización.

### b. Código Fuente Completo y Comentado

```python
from django.core.paginator import Paginator
from django.shortcuts import render
from .models import Producto

def listar_productos(request):
    # Obtener todos los productos
    productos = Producto.objects.all()
    
    # Crear un objeto Paginator con 10 productos por página
    paginator = Paginator(productos, 10)
    
    # Obtener el número de página desde la solicitud GET
    page_number = request.GET.get('page')
    
    # Obtener la página solicitada
    page_obj = paginator.get_page(page_number)
    
    # Renderizar la plantilla con los productos paginados
    return render(request, 'productos/lista.html', {'page_obj': page_obj})
```

### c. Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

En la plantilla `productos/lista.html`, podemos iterar sobre los productos paginados y mostrar los controles de navegación:

```html
{% for producto in page_obj %}
  <div>{{ producto.nombre }} - {{ producto.precio }}</div>
{% endfor %}

<div class="pagination">
  <span class="step-links">
    {% if page_obj.has_previous %}
      <a href="?page=1">&laquo; primera</a>
      <a href="?page={{ page_obj.previous_page_number }}">anterior</a>
    {% endif %}

    <span class="current">
      Página {{ page_obj.number }} de {{ page_obj.paginator.num_pages }}.
    </span>

    {% if page_obj.has_next %}
      <a href="?page={{ page_obj.next_page_number }}">siguiente</a>
      <a href="?page={{ page_obj.paginator.num_pages }}">última &raquo;</a>
    {% endif %}
  </span>
</div>
```

#### Pruebas Unitarias

Para asegurar que la paginación funciona correctamente, podemos escribir pruebas unitarias utilizando el framework de pruebas de Django.

```python
from django.test import TestCase
from django.urls import reverse
from .models import Producto

class PaginacionTestCase(TestCase):
    def setUp(self):
        # Crear 30 productos para probar la paginación
        for i in range(30):
            Producto.objects.create(nombre=f'Producto {i}', precio=10.0)

    def test_paginacion_lista_productos(self):
        response = self.client.get(reverse('listar_productos'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Producto 0')
        self.assertContains(response, 'Producto 9')
        self.assertNotContains(response, 'Producto 10')

        response = self.client.get(reverse('listar_productos') + '?page=2')
        self.assertContains(response, 'Producto 10')
        self.assertContains(response, 'Producto 19')
        self.assertNotContains(response, 'Producto 20')
```

## 4. Mejores Prácticas y Consideraciones de Diseño

### Mejores Prácticas

1. **Limitar el Número de Elementos por Página:** Mantener un número razonable de elementos por página (por ejemplo, 10-20) para asegurar una carga rápida y una buena experiencia de usuario.
2. **Manejo de Errores:** Implementar manejo de errores para casos en los que el número de página solicitado no exista.
3. **Optimización de Consultas:** Utilizar técnicas de optimización como `select_related` y `prefetch_related` para reducir el número de consultas a la base de datos.
4. **Caché:** Considerar el uso de caché para almacenar resultados de paginación y reducir la carga en la base de datos.

### Consideraciones de Diseño

1. **Interfaz de Usuario:** Diseñar controles de navegación que sean intuitivos y accesibles. Considerar el uso de paginación infinita (carga automática de más elementos al desplazarse) si es apropiado para la aplicación.
2. **Seguridad:** Validar y sanitizar los parámetros de paginación para prevenir ataques como la inyección de SQL.
3. **Escalabilidad:** Asegurarse de que la implementación pueda manejar un gran número de elementos sin degradar el rendimiento. Utilizar índices en la base de datos para mejorar la velocidad de las consultas.

### Consejos para la Depuración y Solución de Problemas Comunes

1. **Verificar Parámetros de Paginación:** Asegurarse de que los parámetros de paginación se reciben correctamente en la vista.
2. **Revisar Consultas SQL:** Utilizar herramientas como `django-debug-toolbar` para revisar las consultas SQL generadas y optimizarlas si es necesario.
3. **Pruebas Extensivas:** Realizar pruebas unitarias y de integración para cubrir diferentes escenarios de paginación, incluyendo bordes como la primera y última página.
4. **Monitoreo de Rendimiento:** Utilizar herramientas de monitoreo para identificar y resolver problemas de rendimiento relacionados con la paginación.

Con esta unidad, hemos cubierto de manera exhaustiva la paginación de resultados en listados dentro de un sistema de carrito de compras en Django. La implementación de paginación no solo mejora la experiencia del usuario, sino que también optimiza el rendimiento del sistema, asegurando que la aplicación sea escalable y eficiente.

# Unidad: Compresión de Assets Estáticos

## Introducción y Objetivos de Aprendizaje

En esta unidad, abordaremos la compresión de assets estáticos en el contexto de un sistema de carrito de compras desarrollado con Django. Los assets estáticos incluyen archivos como imágenes, hojas de estilo (CSS) y scripts de JavaScript, que son esenciales para la apariencia y funcionalidad de una aplicación web. La compresión de estos archivos puede mejorar significativamente el rendimiento de la aplicación al reducir el tiempo de carga de las páginas y el uso de ancho de banda.

### Objetivos de Aprendizaje

1. Comprender la importancia de la compresión de assets estáticos en aplicaciones web.
2. Aprender a configurar Django para la compresión de archivos estáticos.
3. Implementar técnicas de compresión utilizando herramientas y librerías específicas.
4. Evaluar el impacto de la compresión en el rendimiento de la aplicación.
5. Aplicar mejores prácticas y consideraciones de diseño para la compresión de assets estáticos.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

La compresión de assets estáticos implica reducir el tamaño de archivos como CSS, JavaScript e imágenes antes de servirlos a los usuarios. Esto se logra mediante técnicas como la minificación y la compresión gzip. La minificación elimina caracteres innecesarios (como espacios y comentarios) del código fuente, mientras que la compresión gzip reduce el tamaño del archivo mediante algoritmos de compresión.

### Casos de Uso

1. **Caso de Uso 1: Minificación de CSS y JavaScript**
   - **Actor**: Desarrollador web.
   - **Descripción**: El desarrollador configura el proyecto Django para minificar los archivos CSS y JavaScript antes de servirlos a los usuarios.
   - **Resultado Esperado**: Los archivos CSS y JavaScript se sirven en su forma minificada, reduciendo el tiempo de carga de la página.

2. **Caso de Uso 2: Compresión Gzip de Assets Estáticos**
   - **Actor**: Administrador del servidor.
   - **Descripción**: El administrador configura el servidor web para servir archivos estáticos comprimidos con gzip.
   - **Resultado Esperado**: Los archivos estáticos se sirven comprimidos, reduciendo el uso de ancho de banda y mejorando el tiempo de carga.

### Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Configuración de Django]
    B --> C[Minificación de CSS y JS]
    C --> D[Configuración del Servidor]
    D --> E[Compresión Gzip]
    E --> F[Servir Assets Comprimidos]
    F --> G[Fin]
```

### Requisitos No Funcionales

1. **Rendimiento**: La compresión de assets estáticos debe mejorar el tiempo de carga de las páginas sin afectar negativamente el rendimiento del servidor.
2. **Escalabilidad**: La solución debe ser escalable para manejar un gran número de solicitudes concurrentes.
3. **Seguridad**: La compresión no debe introducir vulnerabilidades de seguridad en la aplicación.
4. **Mantenibilidad**: La configuración y el código deben ser fáciles de mantener y actualizar.

## Implementación en Python

### Explicación Paso a Paso del Código

Para implementar la compresión de assets estáticos en un proyecto Django, seguiremos estos pasos:

1. **Configuración de Django para Minificación de CSS y JavaScript**: Utilizaremos la librería `django-compressor` para minificar y combinar archivos CSS y JavaScript.
2. **Configuración del Servidor para Compresión Gzip**: Configuraremos el servidor web (por ejemplo, Nginx) para servir archivos comprimidos con gzip.

### Código Fuente Completo y Comentado

#### Paso 1: Instalación de `django-compressor`

Primero, instalamos la librería `django-compressor`:

```bash
pip install django-compressor
```

#### Paso 2: Configuración de `django-compressor` en Django

Modificamos el archivo `settings.py` para incluir la configuración de `django-compressor`:

```python
# settings.py

INSTALLED_APPS = [
    ...
    'compressor',
    ...
]

STATICFILES_FINDERS = [
    ...
    'compressor.finders.CompressorFinder',
    ...
]

COMPRESS_ENABLED = True
COMPRESS_OFFLINE = True
```

#### Paso 3: Uso de `django-compressor` en Plantillas

Modificamos nuestras plantillas para utilizar `django-compressor`:

```html
<!-- base.html -->

{% load compress %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Carrito de Compras</title>
    {% compress css %}
    <link rel="stylesheet" href="{% static 'css/styles.css' %}">
    {% endcompress %}
</head>
<body>
    ...
    {% compress js %}
    <script src="{% static 'js/scripts.js' %}"></script>
    {% endcompress %}
</body>
</html>
```

#### Paso 4: Configuración del Servidor para Compresión Gzip

Configuramos Nginx para servir archivos comprimidos con gzip:

```nginx
# nginx.conf

http {
    ...
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_proxied any;
    gzip_vary on;
    gzip_min_length 256;
    ...
}
```

### Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

Después de configurar `django-compressor` y Nginx, los archivos CSS y JavaScript se servirán minificados y comprimidos. Podemos verificar esto inspeccionando los archivos servidos en el navegador:

1. Abrimos la consola del navegador (F12).
2. Navegamos a la pestaña "Network" (Red).
3. Recargamos la página y observamos los archivos CSS y JavaScript servidos. Deberían estar minificados y comprimidos.

#### Pruebas Unitarias

Podemos escribir pruebas unitarias para verificar que los archivos estáticos se sirven correctamente:

```python
# tests.py

from django.test import TestCase
from django.conf import settings

class StaticFilesCompressionTest(TestCase):
    def test_css_minification(self):
        response = self.client.get(settings.STATIC_URL + 'css/styles.css')
        self.assertEqual(response.status_code, 200)
        self.assertTrue(len(response.content) < 1000)  # Ejemplo de tamaño reducido

    def test_js_minification(self):
        response = self.client.get(settings.STATIC_URL + 'js/scripts.js')
        self.assertEqual(response.status_code, 200)
        self.assertTrue(len(response.content) < 1000)  # Ejemplo de tamaño reducido
```

## Mejores Prácticas y Consideraciones de Diseño

1. **Automatización**: Utilizar herramientas de automatización como `django-compressor` para manejar la minificación y compresión de archivos estáticos.
2. **Monitoreo**: Implementar herramientas de monitoreo para evaluar el impacto de la compresión en el rendimiento de la aplicación.
3. **Actualización Regular**: Mantener las librerías y herramientas de compresión actualizadas para aprovechar las últimas mejoras y correcciones de seguridad.
4. **Pruebas**: Realizar pruebas exhaustivas para asegurarse de que la compresión no introduce errores en la aplicación.
5. **Documentación**: Documentar la configuración y el proceso de compresión para facilitar el mantenimiento y la actualización del sistema.

### Consideraciones de Seguridad

1. **Validación de Entradas**: Asegurarse de que los archivos estáticos no contengan código malicioso.
2. **Configuración del Servidor**: Configurar correctamente el servidor para evitar vulnerabilidades relacionadas con la compresión, como los ataques de descompresión (zip bomb).

### Consideraciones de Rendimiento

1. **Tamaño de los Archivos**: Evaluar regularmente el tamaño de los archivos comprimidos para asegurarse de que la compresión está siendo efectiva.
2. **Carga del Servidor**: Monitorear la carga del servidor para asegurarse de que la compresión no está afectando negativamente el rendimiento.

### Consejos para la Depuración

1. **Verificación Visual**: Utilizar herramientas de desarrollo del navegador para verificar que los archivos están siendo servidos correctamente.
2. **Logs del Servidor**: Revisar los logs del servidor para identificar posibles errores en la configuración de la compresión.
3. **Pruebas Unitarias**: Implementar pruebas unitarias para verificar que los archivos estáticos se sirven correctamente después de la compresión.

Con esto, concluimos la unidad sobre la compresión de assets estáticos en el contexto de un sistema de carrito de compras con Django. Hemos cubierto desde la configuración inicial hasta las mejores prácticas y consideraciones de diseño, proporcionando una guía completa para optimizar el rendimiento de la aplicación mediante la compresión de archivos estáticos.

# Unidad: Tests unitarios con Django's TestCase

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, profundizaremos en el uso de `TestCase` de Django para realizar tests unitarios en el contexto de un sistema de carrito de compras. Los tests unitarios son esenciales para garantizar que cada componente de nuestra aplicación funcione correctamente de forma aislada. Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender la importancia de los tests unitarios en el desarrollo de aplicaciones web.
2. Configurar y utilizar `TestCase` de Django para escribir y ejecutar tests unitarios.
3. Implementar tests unitarios para las funcionalidades clave de un sistema de carrito de compras.
4. Aplicar mejores prácticas en la escritura de tests unitarios.
5. Depurar y solucionar problemas comunes en los tests unitarios.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

El objetivo de esta unidad es proporcionar una guía exhaustiva sobre cómo escribir y ejecutar tests unitarios utilizando `TestCase` de Django. Nos enfocaremos en las funcionalidades clave del sistema de carrito de compras, incluyendo la adición de productos al carrito, la eliminación de productos, y la actualización de cantidades.

### b. Casos de uso

1. **Agregar producto al carrito**:
    - **Descripción**: Verificar que un producto se puede agregar correctamente al carrito.
    - **Precondiciones**: El producto debe existir en la base de datos.
    - **Postcondiciones**: El producto aparece en el carrito con la cantidad especificada.

2. **Eliminar producto del carrito**:
    - **Descripción**: Verificar que un producto se puede eliminar correctamente del carrito.
    - **Precondiciones**: El producto debe estar en el carrito.
    - **Postcondiciones**: El producto ya no aparece en el carrito.

3. **Actualizar cantidad de producto en el carrito**:
    - **Descripción**: Verificar que la cantidad de un producto en el carrito se puede actualizar correctamente.
    - **Precondiciones**: El producto debe estar en el carrito.
    - **Postcondiciones**: La cantidad del producto en el carrito se actualiza a la nueva cantidad especificada.

### c. Diagramas de flujo (en mermaid)

```mermaid
flowchart TD
    A[Inicio] --> B[Agregar producto al carrito]
    B --> C{Producto existe en la BD?}
    C -->|Sí| D[Agregar producto al carrito]
    C -->|No| E[Error: Producto no encontrado]
    D --> F[Fin]
    E --> F

    G[Inicio] --> H[Eliminar producto del carrito]
    H --> I{Producto está en el carrito?}
    I -->|Sí| J[Eliminar producto del carrito]
    I -->|No| K[Error: Producto no está en el carrito]
    J --> L[Fin]
    K --> L

    M[Inicio] --> N[Actualizar cantidad de producto]
    N --> O{Producto está en el carrito?}
    O -->|Sí| P[Actualizar cantidad]
    O -->|No| Q[Error: Producto no está en el carrito]
    P --> R[Fin]
    Q --> R
```

### d. Requisitos no funcionales

1. **Rendimiento**: Los tests unitarios deben ejecutarse rápidamente para no afectar el flujo de desarrollo.
2. **Escalabilidad**: La suite de tests debe ser fácilmente extensible para incluir nuevas funcionalidades.
3. **Mantenibilidad**: El código de los tests debe ser claro y bien documentado para facilitar su mantenimiento.
4. **Seguridad**: Los tests no deben comprometer la seguridad de la aplicación ni exponer datos sensibles.

## Implementación en Python

### a. Explicación paso a paso del código

Para implementar los tests unitarios en Django, utilizaremos la clase `TestCase` de Django, que nos proporciona un entorno de prueba aislado y herramientas útiles para verificar el comportamiento de nuestra aplicación.

#### Paso 1: Configuración inicial

Primero, debemos asegurarnos de que nuestra aplicación esté configurada para ejecutar tests unitarios. Django incluye una configuración predeterminada para esto, pero es importante verificar que todo esté en orden.

#### Paso 2: Creación de la clase de prueba

Crearemos una clase de prueba que herede de `TestCase`. Dentro de esta clase, definiremos métodos de prueba para cada caso de uso identificado.

#### Paso 3: Definición de métodos de prueba

Cada método de prueba debe comenzar con el prefijo `test_` para que Django lo reconozca como un test. Utilizaremos métodos como `self.client.post` y `self.client.get` para simular solicitudes HTTP y verificar las respuestas.

### b. Código fuente completo y comentado

```python
from django.test import TestCase
from django.urls import reverse
from .models import Producto, Carrito

class CarritoTestCase(TestCase):
    
    def setUp(self):
        # Crear productos de prueba
        self.producto1 = Producto.objects.create(nombre="Producto 1", precio=100)
        self.producto2 = Producto.objects.create(nombre="Producto 2", precio=200)
        
    def test_agregar_producto_al_carrito(self):
        # Agregar producto al carrito
        response = self.client.post(reverse('agregar_al_carrito'), {'producto_id': self.producto1.id, 'cantidad': 1})
        self.assertEqual(response.status_code, 200)
        
        # Verificar que el producto está en el carrito
        carrito = Carrito.objects.get()
        self.assertIn(self.producto1, carrito.productos.all())
        self.assertEqual(carrito.productos.get(id=self.producto1.id).cantidad, 1)
    
    def test_eliminar_producto_del_carrito(self):
        # Agregar producto al carrito
        self.client.post(reverse('agregar_al_carrito'), {'producto_id': self.producto1.id, 'cantidad': 1})
        
        # Eliminar producto del carrito
        response = self.client.post(reverse('eliminar_del_carrito'), {'producto_id': self.producto1.id})
        self.assertEqual(response.status_code, 200)
        
        # Verificar que el producto ya no está en el carrito
        carrito = Carrito.objects.get()
        self.assertNotIn(self.producto1, carrito.productos.all())
    
    def test_actualizar_cantidad_producto(self):
        # Agregar producto al carrito
        self.client.post(reverse('agregar_al_carrito'), {'producto_id': self.producto1.id, 'cantidad': 1})
        
        # Actualizar cantidad del producto en el carrito
        response = self.client.post(reverse('actualizar_cantidad'), {'producto_id': self.producto1.id, 'cantidad': 3})
        self.assertEqual(response.status_code, 200)
        
        # Verificar que la cantidad se ha actualizado
        carrito = Carrito.objects.get()
        self.assertEqual(carrito.productos.get(id=self.producto1.id).cantidad, 3)
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso: Agregar producto al carrito

```python
# Simular la adición de un producto al carrito
response = self.client.post(reverse('agregar_al_carrito'), {'producto_id': self.producto1.id, 'cantidad': 1})
assert response.status_code == 200
```

#### Pruebas unitarias

```python
class CarritoTestCase(TestCase):
    
    def setUp(self):
        self.producto1 = Producto.objects.create(nombre="Producto 1", precio=100)
        self.producto2 = Producto.objects.create(nombre="Producto 2", precio=200)
        
    def test_agregar_producto_al_carrito(self):
        response = self.client.post(reverse('agregar_al_carrito'), {'producto_id': self.producto1.id, 'cantidad': 1})
        self.assertEqual(response.status_code, 200)
        carrito = Carrito.objects.get()
        self.assertIn(self.producto1, carrito.productos.all())
        self.assertEqual(carrito.productos.get(id=self.producto1.id).cantidad, 1)
    
    def test_eliminar_producto_del_carrito(self):
        self.client.post(reverse('agregar_al_carrito'), {'producto_id': self.producto1.id, 'cantidad': 1})
        response = self.client.post(reverse('eliminar_del_carrito'), {'producto_id': self.producto1.id})
        self.assertEqual(response.status_code, 200)
        carrito = Carrito.objects.get()
        self.assertNotIn(self.producto1, carrito.productos.all())
    
    def test_actualizar_cantidad_producto(self):
        self.client.post(reverse('agregar_al_carrito'), {'producto_id': self.producto1.id, 'cantidad': 1})
        response = self.client.post(reverse('actualizar_cantidad'), {'producto_id': self.producto1.id, 'cantidad': 3})
        self.assertEqual(response.status_code, 200)
        carrito = Carrito.objects.get()
        self.assertEqual(carrito.productos.get(id=self.producto1.id).cantidad, 3)
```

## Mejores prácticas y consideraciones de diseño

1. **Aislamiento de pruebas**: Cada prueba debe ser independiente y no depender de otras pruebas. Utiliza `setUp` y `tearDown` para preparar y limpiar el entorno de prueba.
2. **Nombres descriptivos**: Utiliza nombres descriptivos para los métodos de prueba para que sea claro qué funcionalidad están verificando.
3. **Cobertura de pruebas**: Asegúrate de que todas las funcionalidades clave estén cubiertas por pruebas unitarias.
4. **Ejecución frecuente**: Ejecuta las pruebas unitarias con frecuencia durante el desarrollo para detectar errores lo antes posible.
5. **Documentación**: Documenta las pruebas para que otros desarrolladores puedan entender fácilmente qué se está verificando y por qué.

### Consideraciones de seguridad

1. **Datos sensibles**: Asegúrate de que los datos sensibles no se expongan en los mensajes de error o en los logs de las pruebas.
2. **Acceso controlado**: Verifica que las pruebas no comprometan la seguridad de la aplicación, especialmente en entornos de producción.

### Rendimiento y escalabilidad

1. **Optimización**: Optimiza las pruebas para que se ejecuten rápidamente, utilizando técnicas como la paralelización si es necesario.
2. **Extensibilidad**: Diseña las pruebas de manera que sea fácil agregar nuevas pruebas a medida que se desarrollan nuevas funcionalidades.

### Depuración y solución de problemas comunes

1. **Mensajes de error claros**: Asegúrate de que los mensajes de error sean claros y proporcionen suficiente información para identificar el problema.
2. **Logs detallados**: Utiliza logs detallados para rastrear el flujo de ejecución y detectar dónde ocurren los errores.
3. **Herramientas de depuración**: Utiliza herramientas de depuración como breakpoints y depuradores interactivos para investigar problemas complejos.

Con esta unidad, los desarrolladores estarán equipados con las habilidades y conocimientos necesarios para escribir y ejecutar tests unitarios efectivos utilizando `TestCase` de Django, asegurando así la calidad y confiabilidad del sistema de carrito de compras.

### Unidad: Tests de integración para flujos principales

#### Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en los tests de integración para los flujos principales de un sistema de carrito de compras desarrollado con Django. Los tests de integración son esenciales para asegurar que los diferentes componentes de una aplicación trabajen juntos como se espera. A diferencia de los tests unitarios, que se centran en probar componentes individuales de manera aislada, los tests de integración verifican la interacción entre múltiples componentes y servicios.

Al finalizar esta unidad, los estudiantes serán capaces de:

1. Comprender la importancia de los tests de integración en el desarrollo de aplicaciones web.
2. Diseñar y escribir tests de integración para los flujos principales de un sistema de carrito de compras.
3. Utilizar herramientas y librerías de Django para implementar y ejecutar tests de integración.
4. Identificar y solucionar problemas comunes que pueden surgir durante la ejecución de tests de integración.
5. Aplicar mejores prácticas y consideraciones de diseño en la implementación de tests de integración.

#### Documento funcional de requerimientos

##### a. Descripción detallada de la funcionalidad

Los tests de integración para los flujos principales del sistema de carrito de compras deben cubrir los siguientes aspectos:

1. **Registro y autenticación de usuarios**: Verificar que los usuarios puedan registrarse, iniciar sesión y cerrar sesión correctamente.
2. **Navegación por el catálogo de productos**: Asegurar que los usuarios puedan navegar por el catálogo de productos, ver detalles de productos individuales y buscar productos específicos.
3. **Gestión del carrito de compras**: Comprobar que los usuarios puedan agregar productos al carrito, actualizar cantidades y eliminar productos del carrito.
4. **Proceso de pago**: Validar que los usuarios puedan proceder al pago, ingresar detalles de envío y pago, y completar la compra.
5. **Historial de pedidos**: Verificar que los usuarios puedan ver el historial de sus pedidos y los detalles de cada pedido.

##### b. Casos de uso

1. **Registro de usuario**:
   - **Actor**: Usuario no autenticado.
   - **Descripción**: El usuario se registra proporcionando un nombre de usuario, correo electrónico y contraseña.
   - **Resultado esperado**: El usuario es redirigido a la página de inicio de sesión y puede iniciar sesión con las credenciales registradas.

2. **Inicio de sesión**:
   - **Actor**: Usuario registrado.
   - **Descripción**: El usuario inicia sesión proporcionando su nombre de usuario y contraseña.
   - **Resultado esperado**: El usuario es redirigido a la página principal y su estado de autenticación es mantenido.

3. **Navegación por el catálogo**:
   - **Actor**: Usuario autenticado o no autenticado.
   - **Descripción**: El usuario navega por el catálogo de productos y visualiza los detalles de un producto específico.
   - **Resultado esperado**: El usuario puede ver la lista de productos y los detalles de cada producto.

4. **Agregar productos al carrito**:
   - **Actor**: Usuario autenticado.
   - **Descripción**: El usuario agrega uno o más productos al carrito de compras.
   - **Resultado esperado**: Los productos seleccionados se agregan al carrito y el usuario puede ver y modificar el contenido del carrito.

5. **Proceso de pago**:
   - **Actor**: Usuario autenticado.
   - **Descripción**: El usuario procede al pago, ingresa detalles de envío y pago, y completa la compra.
   - **Resultado esperado**: La compra se completa exitosamente y el usuario recibe una confirmación del pedido.

6. **Ver historial de pedidos**:
   - **Actor**: Usuario autenticado.
   - **Descripción**: El usuario visualiza el historial de sus pedidos y los detalles de cada pedido.
   - **Resultado esperado**: El usuario puede ver una lista de sus pedidos anteriores y los detalles de cada uno.

##### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Registro de Usuario]
    B --> C[Inicio de Sesión]
    C --> D[Navegación por el Catálogo]
    D --> E[Agregar Productos al Carrito]
    E --> F[Proceso de Pago]
    F --> G[Ver Historial de Pedidos]
    G --> H[Fin]
```

##### d. Requisitos no funcionales

1. **Rendimiento**: Los tests de integración deben ejecutarse en un tiempo razonable para no afectar la productividad del equipo de desarrollo.
2. **Escalabilidad**: Los tests deben ser diseñados de manera que puedan ser fácilmente extendidos para cubrir nuevos flujos y funcionalidades a medida que el sistema evoluciona.
3. **Mantenibilidad**: El código de los tests debe ser claro y bien documentado para facilitar su mantenimiento y actualización.
4. **Seguridad**: Los tests deben asegurar que las interacciones entre componentes no introduzcan vulnerabilidades de seguridad.

#### Implementación en Python

##### a. Explicación paso a paso del código

Para implementar los tests de integración en Django, utilizaremos el módulo `TestCase` de Django y la librería `django.test.Client` para simular solicitudes HTTP y verificar las respuestas del servidor.

1. **Configuración del entorno de pruebas**:
   - Crear un archivo `tests.py` en la aplicación Django correspondiente.
   - Importar las librerías necesarias y configurar el entorno de pruebas.

2. **Escribir tests de integración**:
   - Definir clases de test para cada flujo principal.
   - Implementar métodos de test que simulen las interacciones del usuario y verifiquen los resultados esperados.

3. **Ejecutar y verificar los tests**:
   - Utilizar el comando `python manage.py test` para ejecutar los tests y verificar que todos pasen correctamente.

##### b. Código fuente completo y comentado

```python
# tests.py

from django.test import TestCase, Client
from django.urls import reverse
from django.contrib.auth.models import User
from .models import Product, Order

class IntegrationTests(TestCase):

    def setUp(self):
        # Configuración inicial para los tests
        self.client = Client()
        self.user = User.objects.create_user(username='testuser', password='testpassword')
        self.product = Product.objects.create(name='Test Product', price=10.0, stock=100)

    def test_user_registration(self):
        # Test de registro de usuario
        response = self.client.post(reverse('register'), {
            'username': 'newuser',
            'email': 'newuser@example.com',
            'password1': 'newpassword',
            'password2': 'newpassword'
        })
        self.assertEqual(response.status_code, 302)  # Redirección después del registro
        self.assertTrue(User.objects.filter(username='newuser').exists())

    def test_user_login(self):
        # Test de inicio de sesión
        response = self.client.post(reverse('login'), {
            'username': 'testuser',
            'password': 'testpassword'
        })
        self.assertEqual(response.status_code, 302)  # Redirección después del inicio de sesión
        self.assertTrue(response.wsgi_request.user.is_authenticated)

    def test_product_catalog_navigation(self):
        # Test de navegación por el catálogo de productos
        response = self.client.get(reverse('product_list'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Test Product')

    def test_add_product_to_cart(self):
        # Test de agregar producto al carrito
        self.client.login(username='testuser', password='testpassword')
        response = self.client.post(reverse('add_to_cart', args=[self.product.id]), {
            'quantity': 1
        })
        self.assertEqual(response.status_code, 302)  # Redirección después de agregar al carrito
        self.assertEqual(self.client.session['cart'][str(self.product.id)], 1)

    def test_checkout_process(self):
        # Test del proceso de pago
        self.client.login(username='testuser', password='testpassword')
        self.client.post(reverse('add_to_cart', args=[self.product.id]), {'quantity': 1})
        response = self.client.post(reverse('checkout'), {
            'address': '123 Test St',
            'payment_method': 'credit_card'
        })
        self.assertEqual(response.status_code, 302)  # Redirección después del pago
        self.assertTrue(Order.objects.filter(user=self.user).exists())

    def test_order_history(self):
        # Test de ver historial de pedidos
        self.client.login(username='testuser', password='testpassword')
        Order.objects.create(user=self.user, total=10.0)
        response = self.client.get(reverse('order_history'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, '10.0')

```

##### c. Ejemplos de uso y pruebas unitarias

Para ejecutar los tests de integración, simplemente utiliza el comando `python manage.py test` en la terminal. A continuación se muestra un ejemplo de cómo se vería la salida de los tests:

```
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
......
----------------------------------------------------------------------
Ran 6 tests in 0.123s

OK
Destroying test database for alias 'default'...
```

Esto indica que todos los tests se ejecutaron correctamente y pasaron sin errores.

#### Mejores prácticas y consideraciones de diseño

1. **Aislamiento de tests**: Asegúrate de que cada test sea independiente y no dependa del estado dejado por otros tests. Utiliza el método `setUp` para configurar el entorno antes de cada test y `tearDown` para limpiar después de cada test si es necesario.

2. **Datos de prueba realistas**: Utiliza datos de prueba que sean representativos de los datos reales que manejará la aplicación. Esto ayudará a identificar problemas que podrían surgir en un entorno de producción.

3. **Cobertura de tests**: Asegúrate de que los tests de integración cubran todos los flujos principales y casos de borde. Utiliza herramientas como `coverage.py` para medir la cobertura de los tests y asegurarte de que no haya áreas críticas sin probar.

4. **Documentación y comentarios**: Documenta claramente cada test y agrega comentarios explicativos en el código para facilitar su comprensión y mantenimiento.

5. **Automatización**: Integra la ejecución de tests en el proceso de integración continua (CI) para que se ejecuten automáticamente con cada cambio en el código. Esto ayuda a detectar problemas rápidamente y mantener la calidad del código.

6. **Manejo de errores y excepciones**: Asegúrate de que los tests manejen correctamente los errores y excepciones. Verifica que la aplicación responda de manera adecuada ante situaciones inesperadas.

7. **Optimización del rendimiento**: Si los tests de integración son lentos, identifica y optimiza las áreas que consumen más tiempo. Utiliza técnicas como la paralelización de tests para mejorar el rendimiento.

8. **Revisión y refactorización**: Revisa y refactoriza regularmente los tests para mantener su calidad y relevancia. Elimina tests obsoletos y agrega nuevos tests para cubrir funcionalidades adicionales.

Al seguir estas mejores prácticas y consideraciones de diseño, podrás implementar tests de integración efectivos y mantener la calidad y confiabilidad de tu sistema de carrito de compras con Django.

# Unidad: Uso de factories con Factory Boy

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, exploraremos el uso de factories con la librería Factory Boy en el contexto de un sistema de carrito de compras desarrollado con Django. Factory Boy es una herramienta poderosa para la creación de objetos de prueba, lo que facilita la generación de datos consistentes y realistas para nuestros tests. Al finalizar esta unidad, serás capaz de:

1. Comprender la importancia y el uso de factories en el testing.
2. Configurar y utilizar Factory Boy para crear objetos de prueba en Django.
3. Implementar factories para diferentes modelos de un sistema de carrito de compras.
4. Integrar factories en tus tests unitarios y de integración.
5. Aplicar mejores prácticas y consideraciones de diseño al utilizar Factory Boy.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

Factory Boy es una librería que permite la creación de objetos de prueba de manera sencilla y eficiente. En el contexto de un sistema de carrito de compras, necesitamos generar datos de prueba para diversos modelos, como Usuarios, Productos, Carritos y Órdenes. La funcionalidad principal de Factory Boy es automatizar la creación de estos objetos, permitiendo definir plantillas (factories) que especifican cómo deben ser generados los datos.

### Casos de uso

1. **Generación de datos de prueba para tests unitarios**: Crear objetos de prueba para modelos como Usuario, Producto, Carrito y Orden.
2. **Simulación de escenarios complejos**: Generar datos que simulen escenarios de uso real, como un usuario con múltiples productos en su carrito.
3. **Pruebas de integración**: Utilizar factories para crear datos consistentes y realistas que permitan probar flujos completos del sistema.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Definir modelo]
    B --> C[Crear Factory]
    C --> D[Configurar atributos]
    D --> E[Generar objetos de prueba]
    E --> F[Utilizar en tests]
    F --> G[Verificar resultados]
    G --> H[Fin]
```

### Requisitos no funcionales

1. **Rendimiento**: La generación de datos debe ser rápida y eficiente, sin afectar significativamente el tiempo de ejecución de los tests.
2. **Escalabilidad**: Las factories deben ser fácilmente extensibles para soportar nuevos modelos y atributos a medida que el sistema crece.
3. **Mantenibilidad**: El código de las factories debe ser claro y bien documentado, facilitando su mantenimiento y actualización.

## Implementación en Python

### Explicación paso a paso del código

Para ilustrar el uso de Factory Boy en nuestro sistema de carrito de compras, implementaremos factories para los modelos Usuario, Producto, Carrito y Orden. A continuación, se detalla el proceso de configuración y uso de estas factories.

#### Paso 1: Instalación de Factory Boy

Primero, necesitamos instalar la librería Factory Boy. Esto se puede hacer utilizando pip:

```bash
pip install factory_boy
```

#### Paso 2: Definición de las factories

A continuación, definimos las factories para nuestros modelos. Supondremos que ya tenemos definidos los modelos `Usuario`, `Producto`, `Carrito` y `Orden` en nuestra aplicación Django.

```python
# factories.py
import factory
from django.contrib.auth.models import User
from .models import Producto, Carrito, Orden

class UsuarioFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = User

    username = factory.Faker('user_name')
    email = factory.Faker('email')
    password = factory.PostGenerationMethodCall('set_password', 'password123')

class ProductoFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Producto

    nombre = factory.Faker('word')
    descripcion = factory.Faker('sentence')
    precio = factory.Faker('random_number', digits=5)

class CarritoFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Carrito

    usuario = factory.SubFactory(UsuarioFactory)

class OrdenFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Orden

    carrito = factory.SubFactory(CarritoFactory)
    total = factory.LazyAttribute(lambda o: sum(item.producto.precio for item in o.carrito.items.all()))
```

#### Paso 3: Utilización de las factories en tests

Ahora que hemos definido nuestras factories, podemos utilizarlas en nuestros tests para generar datos de prueba.

```python
# tests.py
from django.test import TestCase
from .factories import UsuarioFactory, ProductoFactory, CarritoFactory, OrdenFactory

class CarritoDeComprasTests(TestCase):

    def test_creacion_usuario(self):
        usuario = UsuarioFactory()
        self.assertIsNotNone(usuario.id)
        self.assertEqual(usuario.username, 'user_name')

    def test_creacion_producto(self):
        producto = ProductoFactory()
        self.assertIsNotNone(producto.id)
        self.assertGreater(producto.precio, 0)

    def test_creacion_carrito(self):
        carrito = CarritoFactory()
        self.assertIsNotNone(carrito.id)
        self.assertIsNotNone(carrito.usuario)

    def test_creacion_orden(self):
        orden = OrdenFactory()
        self.assertIsNotNone(orden.id)
        self.assertGreater(orden.total, 0)
```

### Código fuente completo y comentado

```python
# factories.py
import factory
from django.contrib.auth.models import User
from .models import Producto, Carrito, Orden

# Factory para el modelo Usuario
class UsuarioFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = User

    # Atributos generados con Faker
    username = factory.Faker('user_name')
    email = factory.Faker('email')
    password = factory.PostGenerationMethodCall('set_password', 'password123')

# Factory para el modelo Producto
class ProductoFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Producto

    # Atributos generados con Faker
    nombre = factory.Faker('word')
    descripcion = factory.Faker('sentence')
    precio = factory.Faker('random_number', digits=5)

# Factory para el modelo Carrito
class CarritoFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Carrito

    # Relación con el modelo Usuario
    usuario = factory.SubFactory(UsuarioFactory)

# Factory para el modelo Orden
class OrdenFactory(factory.django.DjangoModelFactory):
    class Meta:
        model = Orden

    # Relación con el modelo Carrito
    carrito = factory.SubFactory(CarritoFactory)
    # Cálculo del total basado en los productos del carrito
    total = factory.LazyAttribute(lambda o: sum(item.producto.precio for item in o.carrito.items.all()))

# tests.py
from django.test import TestCase
from .factories import UsuarioFactory, ProductoFactory, CarritoFactory, OrdenFactory

# Tests para el sistema de carrito de compras
class CarritoDeComprasTests(TestCase):

    # Test para la creación de un usuario
    def test_creacion_usuario(self):
        usuario = UsuarioFactory()
        self.assertIsNotNone(usuario.id)
        self.assertEqual(usuario.username, 'user_name')

    # Test para la creación de un producto
    def test_creacion_producto(self):
        producto = ProductoFactory()
        self.assertIsNotNone(producto.id)
        self.assertGreater(producto.precio, 0)

    # Test para la creación de un carrito
    def test_creacion_carrito(self):
        carrito = CarritoFactory()
        self.assertIsNotNone(carrito.id)
        self.assertIsNotNone(carrito.usuario)

    # Test para la creación de una orden
    def test_creacion_orden(self):
        orden = OrdenFactory()
        self.assertIsNotNone(orden.id)
        self.assertGreater(orden.total, 0)
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplo 1: Creación de un usuario

```python
# Crear un usuario utilizando la factory
usuario = UsuarioFactory()
print(usuario.username)  # Salida: user_name
print(usuario.email)     # Salida: email@example.com
```

#### Ejemplo 2: Creación de un producto

```python
# Crear un producto utilizando la factory
producto = ProductoFactory()
print(producto.nombre)       # Salida: nombre del producto
print(producto.precio)       # Salida: precio del producto
```

#### Ejemplo 3: Creación de un carrito

```python
# Crear un carrito utilizando la factory
carrito = CarritoFactory()
print(carrito.usuario.username)  # Salida: user_name
```

#### Ejemplo 4: Creación de una orden

```python
# Crear una orden utilizando la factory
orden = OrdenFactory()
print(orden.total)  # Salida: total de la orden
```

### Pruebas unitarias

Las pruebas unitarias ya se han incluido en la sección anterior. A continuación, se muestra cómo ejecutarlas:

```bash
python manage.py test
```

## Mejores prácticas y consideraciones de diseño

1. **Reutilización de código**: Las factories permiten reutilizar código de manera eficiente, evitando la duplicación de lógica de creación de objetos en múltiples tests.
2. **Consistencia en los datos**: Utilizar factories garantiza que los datos de prueba sean consistentes y realistas, lo que mejora la fiabilidad de los tests.
3. **Modularidad**: Definir factories en módulos separados facilita su mantenimiento y actualización.
4. **Extensibilidad**: Las factories deben ser fácilmente extensibles para soportar nuevos modelos y atributos a medida que el sistema crece.
5. **Documentación**: Es importante documentar las factories y su uso para facilitar la comprensión y el mantenimiento del código.

### Consideraciones sobre seguridad, rendimiento y escalabilidad

1. **Seguridad**: Asegúrate de que los datos generados por las factories no incluyan información sensible o real. Utiliza Faker para generar datos ficticios.
2. **Rendimiento**: La generación de datos debe ser rápida y eficiente. Evita la creación de objetos innecesarios y utiliza técnicas como `LazyAttribute` para cálculos diferidos.
3. **Escalabilidad**: Diseña las factories de manera que puedan ser fácilmente extendidas para soportar nuevos modelos y atributos sin afectar el rendimiento.

### Consejos para la depuración y solución de problemas comunes

1. **Errores en la generación de datos**: Si encuentras errores al generar datos con las factories, revisa los atributos y métodos utilizados. Asegúrate de que todos los campos obligatorios estén definidos.
2. **Problemas de rendimiento**: Si la generación de datos es lenta, revisa el uso de `LazyAttribute` y otras técnicas para optimizar el rendimiento.
3. **Inconsistencias en los datos**: Si los datos generados no son consistentes, revisa las definiciones de las factories y asegúrate de que los atributos dependientes estén correctamente configurados.

Con esto concluimos la unidad sobre el uso de factories con Factory Boy en el contexto de un sistema de carrito de compras con Django. Ahora estás equipado con las herramientas y conocimientos necesarios para generar datos de prueba de manera eficiente y efectiva.

# Unidad: Cobertura de código con coverage.py

## 1. Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la cobertura de código utilizando la herramienta `coverage.py` dentro del contexto de un sistema de carrito de compras desarrollado con Django. La cobertura de código es una métrica esencial en el desarrollo de software que nos permite evaluar qué partes de nuestro código están siendo ejecutadas por nuestros tests. Un alto porcentaje de cobertura de código no garantiza la ausencia de errores, pero sí proporciona una mayor confianza en la calidad del software.

### Objetivos de aprendizaje

1. Comprender la importancia de la cobertura de código en el desarrollo de software.
2. Aprender a instalar y configurar `coverage.py` en un proyecto Django.
3. Ejecutar y analizar reportes de cobertura de código.
4. Integrar `coverage.py` con herramientas de CI/CD.
5. Aplicar mejores prácticas para mejorar la cobertura de código.

## 2. Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es implementar y utilizar `coverage.py` para medir la cobertura de código en un sistema de carrito de compras desarrollado con Django. Esto incluye la instalación y configuración de `coverage.py`, la ejecución de tests para generar reportes de cobertura, y el análisis de dichos reportes para identificar áreas del código que requieren más pruebas.

### b. Casos de uso

1. **Caso de uso 1: Instalación y configuración de coverage.py**
   - **Actor**: Desarrollador
   - **Descripción**: El desarrollador instala y configura `coverage.py` en el entorno de desarrollo del proyecto Django.
   - **Precondiciones**: El proyecto Django está correctamente configurado y funcional.
   - **Postcondiciones**: `coverage.py` está instalado y configurado para su uso en el proyecto.

2. **Caso de uso 2: Ejecución de tests y generación de reportes de cobertura**
   - **Actor**: Desarrollador
   - **Descripción**: El desarrollador ejecuta los tests del proyecto utilizando `coverage.py` y genera un reporte de cobertura.
   - **Precondiciones**: `coverage.py` está instalado y configurado.
   - **Postcondiciones**: Se genera un reporte de cobertura que muestra qué partes del código fueron ejecutadas por los tests.

3. **Caso de uso 3: Análisis de reportes de cobertura**
   - **Actor**: Desarrollador
   - **Descripción**: El desarrollador analiza el reporte de cobertura para identificar áreas del código que no están cubiertas por los tests.
   - **Precondiciones**: Se ha generado un reporte de cobertura.
   - **Postcondiciones**: El desarrollador identifica áreas del código que requieren más pruebas y planifica la creación de tests adicionales.

### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Instalación de coverage.py]
    B --> C[Configuración de coverage.py]
    C --> D[Ejecución de tests con coverage.py]
    D --> E[Generación de reporte de cobertura]
    E --> F[Análisis del reporte de cobertura]
    F --> G[Identificación de áreas no cubiertas]
    G --> H[Creación de tests adicionales]
    H --> I[Reejecución de tests]
    I --> J[Fin]
```

### d. Requisitos no funcionales

1. **Rendimiento**: La ejecución de `coverage.py` no debe afectar significativamente el tiempo de ejecución de los tests.
2. **Escalabilidad**: La herramienta debe ser capaz de manejar proyectos de gran tamaño sin problemas de rendimiento.
3. **Seguridad**: La configuración de `coverage.py` no debe introducir vulnerabilidades de seguridad en el proyecto.
4. **Compatibilidad**: `coverage.py` debe ser compatible con la versión de Django y Python utilizada en el proyecto.
5. **Mantenibilidad**: La configuración y uso de `coverage.py` debe ser documentada claramente para facilitar su mantenimiento y actualización.

## 3. Implementación en Python

### a. Explicación paso a paso del código

#### Paso 1: Instalación de coverage.py

Para instalar `coverage.py`, utilizamos `pip`, el gestor de paquetes de Python. Ejecutamos el siguiente comando en la terminal:

```bash
pip install coverage
```

#### Paso 2: Configuración de coverage.py

Una vez instalado, debemos configurar `coverage.py` para que funcione correctamente con nuestro proyecto Django. Creamos un archivo `.coveragerc` en el directorio raíz del proyecto con la siguiente configuración:

```ini
[run]
source = .
omit = 
    */migrations/*
    */tests/*
    manage.py

[report]
show_missing = True
```

Esta configuración indica que queremos medir la cobertura de todo el código fuente del proyecto, excluyendo las migraciones y los archivos de tests, y que queremos que se muestren las líneas de código que no están cubiertas por los tests.

#### Paso 3: Ejecución de tests con coverage.py

Para ejecutar los tests del proyecto utilizando `coverage.py`, utilizamos el siguiente comando:

```bash
coverage run manage.py test
```

Este comando ejecuta los tests del proyecto y recopila datos de cobertura.

#### Paso 4: Generación de reportes de cobertura

Una vez ejecutados los tests, generamos un reporte de cobertura con el siguiente comando:

```bash
coverage report -m
```

Este comando muestra un reporte en la terminal con el porcentaje de cobertura de cada archivo y las líneas de código que no están cubiertas.

Para generar un reporte en formato HTML, utilizamos el siguiente comando:

```bash
coverage html
```

Este comando crea un directorio `htmlcov` con un reporte en formato HTML que podemos abrir en un navegador para una visualización más detallada.

### b. Código fuente completo y comentado

A continuación, se presenta el código fuente completo y comentado para la configuración y uso de `coverage.py` en un proyecto Django.

```python
# Instalación de coverage.py
# Ejecutar en la terminal:
# pip install coverage

# Configuración de coverage.py
# Crear un archivo .coveragerc en el directorio raíz del proyecto con el siguiente contenido:
"""
[run]
source = .
omit = 
    */migrations/*
    */tests/*
    manage.py

[report]
show_missing = True
"""

# Ejecución de tests con coverage.py
# Ejecutar en la terminal:
# coverage run manage.py test

# Generación de reportes de cobertura
# Para un reporte en la terminal:
# coverage report -m

# Para un reporte en formato HTML:
# coverage html
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Supongamos que tenemos un proyecto Django con una aplicación llamada `shop` que contiene el siguiente archivo `models.py`:

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=255)
    price = models.DecimalField(max_digits=10, decimal_places=2)
```

Y el siguiente archivo `tests.py`:

```python
from django.test import TestCase
from .models import Product

class ProductTestCase(TestCase):
    def test_product_creation(self):
        product = Product.objects.create(name="Test Product", price=10.00)
        self.assertEqual(product.name, "Test Product")
        self.assertEqual(product.price, 10.00)
```

Para medir la cobertura de código, seguimos los pasos descritos anteriormente:

1. Instalamos `coverage.py`:
   ```bash
   pip install coverage
   ```

2. Configuramos `coverage.py` creando el archivo `.coveragerc`:

   ```ini
   [run]
   source = .
   omit = 
       */migrations/*
       */tests/*
       manage.py

   [report]
   show_missing = True
   ```

3. Ejecutamos los tests con `coverage.py`:
   ```bash
   coverage run manage.py test
   ```

4. Generamos un reporte de cobertura en la terminal:
   ```bash
   coverage report -m
   ```

5. Generamos un reporte de cobertura en formato HTML:
   ```bash
   coverage html
   ```

#### Pruebas unitarias

Para asegurar que `coverage.py` está funcionando correctamente, podemos escribir pruebas unitarias que verifiquen la generación de reportes de cobertura. Sin embargo, dado que `coverage.py` es una herramienta externa, las pruebas unitarias se enfocarán en verificar que los comandos se ejecutan correctamente y que los reportes se generan.

```python
import os
import subprocess
from django.test import TestCase

class CoverageTestCase(TestCase):
    def test_coverage_report_generation(self):
        # Ejecutar los tests con coverage.py
        result = subprocess.run(['coverage', 'run', 'manage.py', 'test'], capture_output=True, text=True)
        self.assertEqual(result.returncode, 0, msg="Coverage run failed")

        # Generar el reporte de cobertura en la terminal
        result = subprocess.run(['coverage', 'report', '-m'], capture_output=True, text=True)
        self.assertEqual(result.returncode, 0, msg="Coverage report generation failed")

        # Generar el reporte de cobertura en formato HTML
        result = subprocess.run(['coverage', 'html'], capture_output=True, text=True)
        self.assertEqual(result.returncode, 0, msg="Coverage HTML report generation failed")

        # Verificar que el directorio htmlcov fue creado
        self.assertTrue(os.path.isdir('htmlcov'), msg="htmlcov directory not created")
```

## 4. Mejores prácticas y consideraciones de diseño

### Mejores prácticas

1. **Configurar correctamente `coverage.py`**: Asegúrate de que el archivo `.coveragerc` excluya archivos que no son relevantes para la cobertura, como migraciones y archivos de tests.
2. **Ejecutar cobertura de código regularmente**: Integra `coverage.py` en tu flujo de trabajo de CI/CD para asegurarte de que la cobertura de código se mide en cada commit.
3. **Analizar los reportes de cobertura**: Revisa regularmente los reportes de cobertura para identificar áreas del código que no están cubiertas por los tests y planifica la creación de tests adicionales.
4. **No obsesionarse con el 100% de cobertura**: Aunque un alto porcentaje de cobertura es deseable, no es necesario alcanzar el 100%. Enfócate en cubrir las partes críticas del código y en escribir tests significativos.

### Consideraciones de diseño

1. **Modularidad**: Diseña tu código de manera modular para facilitar la escritura de tests y la medición de cobertura.
2. **Simplicidad**: Mantén tu configuración de `coverage.py` simple y fácil de entender. Documenta cualquier configuración adicional que sea necesaria.
3. **Documentación**: Documenta el uso de `coverage.py` en tu proyecto para que otros desarrolladores puedan entender cómo utilizarlo y mantenerlo.
4. **Rendimiento**: Asegúrate de que la ejecución de `coverage.py` no afecte significativamente el tiempo de ejecución de los tests. Si es necesario, optimiza tus tests para mejorar el rendimiento.

### Consejos para la depuración y solución de problemas comunes

1. **Problema: `coverage.py` no mide la cobertura de algunos archivos**
   - **Solución**: Verifica la configuración en el archivo `.coveragerc` para asegurarte de que los archivos relevantes están incluidos en la medición de cobertura.

2. **Problema: La generación del reporte HTML falla**
   - **Solución**: Asegúrate de que tienes permisos de escritura en el directorio donde se generará el reporte. Verifica también que `coverage.py` está correctamente instalado.

3. **Problema: La cobertura de código es baja**
   - **Solución**: Revisa los reportes de cobertura para identificar áreas del código que no están cubiertas por los tests. Escribe tests adicionales para cubrir estas áreas.

4. **Problema: `coverage.py` afecta el rendimiento de los tests**
   - **Solución**: Optimiza tus tests para mejorar el rendimiento. Considera ejecutar `coverage.py` solo en entornos de CI/CD si el impacto en el rendimiento es significativo.

Al seguir estas mejores prácticas y consideraciones de diseño, podrás utilizar `coverage.py` de manera efectiva para medir y mejorar la cobertura de código en tu proyecto Django, asegurando así una mayor calidad y confiabilidad del software.

# Unidad: Tests de rendimiento

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en los tests de rendimiento, una parte crucial del proceso de desarrollo de software que asegura que nuestra aplicación web, en este caso un sistema de carrito de compras desarrollado con Django, funcione de manera eficiente bajo diversas condiciones de carga. Los tests de rendimiento nos permiten identificar cuellos de botella y optimizar el rendimiento antes de que los usuarios finales experimenten problemas.

### Objetivos de aprendizaje

1. Comprender la importancia de los tests de rendimiento en el desarrollo de aplicaciones web.
2. Aprender a diseñar y ejecutar tests de rendimiento utilizando herramientas específicas.
3. Implementar tests de rendimiento en un sistema de carrito de compras con Django.
4. Analizar los resultados de los tests de rendimiento y aplicar mejoras basadas en estos resultados.
5. Conocer las mejores prácticas y consideraciones de diseño para optimizar el rendimiento de aplicaciones web.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

Los tests de rendimiento se centran en evaluar cómo se comporta el sistema bajo diferentes condiciones de carga. Para nuestro sistema de carrito de compras, esto implica simular múltiples usuarios realizando diversas acciones como navegar por productos, agregar productos al carrito, y realizar compras. Los tests de rendimiento deben medir métricas clave como el tiempo de respuesta, el uso de CPU y memoria, y la tasa de errores.

### Casos de uso

1. **Simulación de múltiples usuarios navegando por productos**:
   - Usuarios concurrentes acceden a la página de listado de productos.
   - Medición del tiempo de carga de la página y uso de recursos.

2. **Simulación de usuarios agregando productos al carrito**:
   - Usuarios concurrentes seleccionan productos y los agregan al carrito.
   - Medición del tiempo de respuesta y uso de recursos durante la operación.

3. **Simulación de usuarios realizando compras**:
   - Usuarios concurrentes completan el proceso de compra.
   - Medición del tiempo de respuesta, uso de recursos y tasa de éxito de las transacciones.

### Diagramas de flujo (en mermaid)

```mermaid
graph TD;
    A[Inicio] --> B[Simulación de usuarios navegando por productos]
    B --> C[Medición del tiempo de carga y uso de recursos]
    C --> D[Simulación de usuarios agregando productos al carrito]
    D --> E[Medición del tiempo de respuesta y uso de recursos]
    E --> F[Simulación de usuarios realizando compras]
    F --> G[Medición del tiempo de respuesta, uso de recursos y tasa de éxito]
    G --> H[Análisis de resultados]
    H --> I[Aplicación de mejoras]
    I --> J[Fin]
```

### Requisitos no funcionales

1. **Escalabilidad**: Los tests deben ser capaces de simular un número creciente de usuarios para evaluar cómo escala la aplicación.
2. **Reproducibilidad**: Los tests deben ser reproducibles para permitir comparaciones entre diferentes ejecuciones.
3. **Automatización**: Los tests deben ser automatizables para integrarse en el pipeline de CI/CD.
4. **Reportabilidad**: Los resultados de los tests deben ser fácilmente interpretables y presentables en forma de informes.

## Implementación en Python

### Explicación paso a paso del código

Para implementar tests de rendimiento en nuestro sistema de carrito de compras con Django, utilizaremos la herramienta `Locust`, que es una herramienta de carga distribuida y de código abierto que permite definir el comportamiento de los usuarios en Python.

#### Instalación de Locust

Primero, necesitamos instalar Locust. Esto se puede hacer fácilmente utilizando `pip`:

```bash
pip install locust
```

#### Definición de los tests de rendimiento

Crearemos un archivo llamado `locustfile.py` en el directorio raíz de nuestro proyecto Django. Este archivo contendrá la definición de los tests de rendimiento.

1. **Importaciones y configuración inicial**:
   ```python
   from locust import HttpUser, TaskSet, task, between

   class WebsiteTasks(TaskSet):
       @task(1)
       def index(self):
           self.client.get("/")

       @task(2)
       def view_product(self):
           self.client.get("/product/1/")

       @task(3)
       def add_to_cart(self):
           self.client.post("/cart/add/", {"product_id": 1, "quantity": 1})

       @task(4)
       def checkout(self):
           self.client.post("/checkout/", {"address": "123 Main St", "payment_method": "card"})

   class WebsiteUser(HttpUser):
       tasks = [WebsiteTasks]
       wait_time = between(1, 5)
   ```

2. **Explicación del código**:
   - **Importaciones**: Importamos las clases y funciones necesarias de Locust.
   - **WebsiteTasks**: Definimos una clase `WebsiteTasks` que hereda de `TaskSet`. Esta clase contiene las tareas que los usuarios realizarán.
     - **index**: Simula la navegación a la página principal.
     - **view_product**: Simula la visualización de un producto específico.
     - **add_to_cart**: Simula la acción de agregar un producto al carrito.
     - **checkout**: Simula el proceso de compra.
   - **WebsiteUser**: Definimos una clase `WebsiteUser` que hereda de `HttpUser`. Esta clase representa a un usuario que realiza las tareas definidas en `WebsiteTasks`. La propiedad `wait_time` define el tiempo de espera entre tareas.

### Código fuente completo y comentado

```python
from locust import HttpUser, TaskSet, task, between

class WebsiteTasks(TaskSet):
    @task(1)
    def index(self):
        """
        Simula la navegación a la página principal.
        """
        self.client.get("/")

    @task(2)
    def view_product(self):
        """
        Simula la visualización de un producto específico.
        """
        self.client.get("/product/1/")

    @task(3)
    def add_to_cart(self):
        """
        Simula la acción de agregar un producto al carrito.
        """
        self.client.post("/cart/add/", {"product_id": 1, "quantity": 1})

    @task(4)
    def checkout(self):
        """
        Simula el proceso de compra.
        """
        self.client.post("/checkout/", {"address": "123 Main St", "payment_method": "card"})

class WebsiteUser(HttpUser):
    tasks = [WebsiteTasks]
    wait_time = between(1, 5)
```

### Ejemplos de uso y pruebas unitarias

Para ejecutar los tests de rendimiento, utilizamos el siguiente comando en la terminal:

```bash
locust -f locustfile.py
```

Esto iniciará una interfaz web en `http://localhost:8089` donde podemos configurar el número de usuarios simulados y la tasa de generación de usuarios.

#### Ejemplo de configuración:

- **Número de usuarios simulados**: 100
- **Tasa de generación de usuarios**: 10 usuarios por segundo

Al iniciar la prueba, Locust comenzará a simular los usuarios realizando las tareas definidas y mostrará métricas en tiempo real como el tiempo de respuesta, el número de solicitudes por segundo, y la tasa de errores.

### Análisis de resultados

Una vez finalizada la prueba, Locust proporciona un informe detallado con las métricas recopiladas. Es importante analizar estos resultados para identificar posibles cuellos de botella y áreas de mejora. Por ejemplo, si el tiempo de respuesta para agregar productos al carrito es significativamente mayor que el tiempo de respuesta para navegar por productos, esto podría indicar un problema en la lógica de agregación al carrito que necesita ser optimizado.

## Mejores prácticas y consideraciones de diseño

1. **Planificación de pruebas**: Antes de ejecutar los tests de rendimiento, es crucial definir claramente los objetivos y las métricas que se desean evaluar. Esto incluye identificar las rutas críticas de la aplicación y los escenarios de uso más comunes.

2. **Simulación realista**: Asegúrate de que los tests simulen de manera realista el comportamiento de los usuarios. Esto incluye variar los tiempos de espera entre acciones y simular diferentes tipos de usuarios con distintos patrones de uso.

3. **Automatización**: Integra los tests de rendimiento en el pipeline de CI/CD para que se ejecuten automáticamente en cada despliegue. Esto ayuda a identificar problemas de rendimiento de manera temprana y continua.

4. **Monitoreo y alertas**: Implementa herramientas de monitoreo y alertas para detectar problemas de rendimiento en tiempo real. Esto permite una respuesta rápida a problemas que puedan surgir en producción.

5. **Optimización iterativa**: Utiliza los resultados de los tests de rendimiento para realizar optimizaciones iterativas. Esto incluye ajustar la configuración del servidor, optimizar consultas a la base de datos, y mejorar la eficiencia del código.

6. **Documentación**: Documenta los resultados de los tests de rendimiento y las optimizaciones realizadas. Esto facilita la comprensión del historial de rendimiento de la aplicación y ayuda en la toma de decisiones futuras.

### Consideraciones sobre seguridad, rendimiento y escalabilidad

- **Seguridad**: Asegúrate de que los tests de rendimiento no comprometan la seguridad de la aplicación. Esto incluye evitar la exposición de datos sensibles y asegurar que los endpoints utilizados en las pruebas estén protegidos adecuadamente.

- **Rendimiento**: Optimiza el código y la infraestructura para manejar la carga esperada. Esto puede incluir el uso de cachés, balanceadores de carga, y optimización de consultas a la base de datos.

- **Escalabilidad**: Diseña la aplicación para escalar horizontalmente, permitiendo agregar más servidores para manejar una mayor carga. Los tests de rendimiento deben incluir escenarios de escalabilidad para evaluar cómo se comporta la aplicación bajo una carga creciente.

### Consejos para la depuración y solución de problemas comunes

1. **Identificación de cuellos de botella**: Utiliza las métricas de los tests de rendimiento para identificar cuellos de botella en la aplicación. Esto puede incluir tiempos de respuesta elevados, alto uso de CPU o memoria, y tasas de error elevadas.

2. **Optimización de consultas a la base de datos**: Las consultas a la base de datos son a menudo una fuente de problemas de rendimiento. Utiliza herramientas de perfilado para identificar consultas lentas y optimizarlas.

3. **Uso de cachés**: Implementa cachés para reducir la carga en la base de datos y mejorar los tiempos de respuesta. Esto puede incluir el uso de cachés en memoria como Redis o Memcached.

4. **Balanceo de carga**: Utiliza balanceadores de carga para distribuir la carga entre múltiples servidores. Esto ayuda a mejorar la disponibilidad y el rendimiento de la aplicación.

5. **Monitoreo continuo**: Implementa herramientas de monitoreo para realizar un seguimiento continuo del rendimiento de la aplicación. Esto ayuda a detectar problemas de manera proactiva y tomar medidas correctivas antes de que afecten a los usuarios finales.

En resumen, los tests de rendimiento son una parte esencial del proceso de desarrollo de software que nos permite asegurar que nuestra aplicación funcione de manera eficiente bajo diversas condiciones de carga. Al seguir las mejores prácticas y utilizar herramientas adecuadas, podemos identificar y resolver problemas de rendimiento de manera efectiva, asegurando una experiencia de usuario óptima.

# Unidad: Configuración de Gunicorn y Nginx

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la configuración de Gunicorn y Nginx para el despliegue de un sistema de carrito de compras desarrollado con Django. Gunicorn es un servidor WSGI para aplicaciones web en Python, mientras que Nginx es un servidor web y proxy inverso que se utiliza para manejar solicitudes HTTP y servir archivos estáticos. Al final de esta unidad, serás capaz de:

1. Configurar Gunicorn para servir una aplicación Django.
2. Configurar Nginx como un proxy inverso para Gunicorn.
3. Entender cómo optimizar y asegurar la configuración de Gunicorn y Nginx.
4. Realizar pruebas para asegurar que la configuración funciona correctamente.

## Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es configurar Gunicorn y Nginx para desplegar un sistema de carrito de compras desarrollado con Django. Esto incluye:

1. **Configuración de Gunicorn**: Preparar Gunicorn para servir la aplicación Django.
2. **Configuración de Nginx**: Configurar Nginx como un proxy inverso que redirige las solicitudes HTTP a Gunicorn.
3. **Optimización y seguridad**: Ajustar las configuraciones para mejorar el rendimiento y la seguridad del sistema.

### b. Casos de uso

1. **Desarrollador despliega la aplicación**: Un desarrollador configura Gunicorn y Nginx para desplegar la aplicación Django en un servidor de producción.
2. **Usuario accede a la aplicación**: Un usuario accede a la aplicación web a través de un navegador, y Nginx redirige la solicitud a Gunicorn, que sirve la aplicación Django.
3. **Administrador ajusta configuraciones**: Un administrador del sistema ajusta las configuraciones de Gunicorn y Nginx para mejorar el rendimiento y la seguridad.

### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Usuario accede a la aplicación] --> B[Nginx recibe la solicitud]
    B --> C[Nginx redirige la solicitud a Gunicorn]
    C --> D[Gunicorn sirve la aplicación Django]
    D --> E[Nginx envía la respuesta al usuario]
```

### d. Requisitos no funcionales

1. **Rendimiento**: La configuración debe ser capaz de manejar múltiples solicitudes concurrentes sin degradar el rendimiento.
2. **Seguridad**: La configuración debe proteger la aplicación contra ataques comunes como DDoS y SQL injection.
3. **Escalabilidad**: La configuración debe permitir la fácil escalabilidad de la aplicación para manejar un aumento en el tráfico.
4. **Mantenibilidad**: La configuración debe ser fácil de mantener y actualizar.

## Implementación en Python

### a. Explicación paso a paso del código

#### 1. Instalación de Gunicorn y Nginx

Primero, debemos instalar Gunicorn y Nginx en nuestro servidor. Esto se puede hacer utilizando el gestor de paquetes `apt` en sistemas basados en Debian (como Ubuntu).

```bash
sudo apt update
sudo apt install gunicorn nginx
```

#### 2. Configuración de Gunicorn

Creamos un archivo de configuración para Gunicorn que especifica los detalles de cómo debe servir nuestra aplicación Django. Guardamos este archivo como `gunicorn_config.py`.

```python
# gunicorn_config.py

command = '/usr/local/bin/gunicorn'
pythonpath = '/path/to/your/project'
bind = '127.0.0.1:8000'
workers = 3
user = 'youruser'
```

#### 3. Configuración de Nginx

Creamos un archivo de configuración para Nginx que especifica cómo debe manejar las solicitudes y redirigirlas a Gunicorn. Guardamos este archivo en `/etc/nginx/sites-available/yourproject`.

```nginx
# /etc/nginx/sites-available/yourproject

server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /path/to/your/project/static/;
    }

    location /media/ {
        alias /path/to/your/project/media/;
    }
}
```

Luego, creamos un enlace simbólico en `/etc/nginx/sites-enabled/` para habilitar esta configuración.

```bash
sudo ln -s /etc/nginx/sites-available/yourproject /etc/nginx/sites-enabled/
```

#### 4. Iniciar y habilitar los servicios

Iniciamos y habilitamos los servicios de Gunicorn y Nginx para que se ejecuten al iniciar el sistema.

```bash
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
sudo systemctl restart nginx
```

### b. Código fuente completo y comentado

A continuación, se presenta el código completo y comentado para la configuración de Gunicorn y Nginx:

#### gunicorn_config.py

```python
# gunicorn_config.py

# Comando para ejecutar Gunicorn
command = '/usr/local/bin/gunicorn'

# Ruta al directorio del proyecto Django
pythonpath = '/path/to/your/project'

# Dirección y puerto en los que Gunicorn escuchará
bind = '127.0.0.1:8000'

# Número de trabajadores (procesos) que Gunicorn utilizará
workers = 3

# Usuario bajo el cual se ejecutará Gunicorn
user = 'youruser'
```

#### Configuración de Nginx

```nginx
# /etc/nginx/sites-available/yourproject

# Configuración del servidor
server {
    # Puerto en el que Nginx escuchará
    listen 80;

    # Nombre del servidor (dominio)
    server_name yourdomain.com;

    # Configuración para redirigir solicitudes a Gunicorn
    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Configuración para servir archivos estáticos
    location /static/ {
        alias /path/to/your/project/static/;
    }

    # Configuración para servir archivos multimedia
    location /media/ {
        alias /path/to/your/project/media/;
    }
}
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Una vez configurados Gunicorn y Nginx, podemos acceder a nuestra aplicación Django a través del navegador utilizando el dominio configurado (`yourdomain.com`). Nginx redirigirá la solicitud a Gunicorn, que servirá la aplicación Django.

#### Pruebas unitarias

Para asegurar que la configuración funciona correctamente, podemos realizar las siguientes pruebas:

1. **Prueba de acceso**: Acceder a la aplicación a través del navegador y verificar que se carga correctamente.
2. **Prueba de archivos estáticos**: Acceder a un archivo estático (por ejemplo, una imagen) y verificar que se carga correctamente.
3. **Prueba de archivos multimedia**: Acceder a un archivo multimedia (por ejemplo, un archivo subido por el usuario) y verificar que se carga correctamente.
4. **Prueba de carga**: Utilizar una herramienta de prueba de carga (como `ab` o `siege`) para simular múltiples solicitudes concurrentes y verificar que la aplicación maneja la carga sin problemas.

### d. Mejores prácticas y consideraciones de diseño

#### Optimización del rendimiento

1. **Ajuste de trabajadores**: El número de trabajadores en Gunicorn debe ajustarse según los recursos del servidor y la carga esperada. Una regla general es utilizar 2-4 trabajadores por núcleo de CPU.
2. **Compresión**: Habilitar la compresión en Nginx para reducir el tamaño de las respuestas y mejorar el tiempo de carga.
3. **Caching**: Configurar el caching en Nginx para reducir la carga en el servidor y mejorar el rendimiento.

#### Seguridad

1. **TLS/SSL**: Configurar TLS/SSL en Nginx para asegurar las comunicaciones entre el servidor y los usuarios.
2. **Cabeceras de seguridad**: Configurar cabeceras de seguridad en Nginx (como `Content-Security-Policy` y `X-Content-Type-Options`) para proteger contra ataques comunes.
3. **Firewall**: Utilizar un firewall para proteger el servidor contra accesos no autorizados.

#### Escalabilidad

1. **Balanceo de carga**: Utilizar un balanceador de carga para distribuir las solicitudes entre múltiples instancias de Gunicorn.
2. **Autoescalado**: Configurar autoescalado para añadir o remover instancias de Gunicorn según la carga del servidor.

#### Mantenibilidad

1. **Documentación**: Mantener una documentación clara y detallada de la configuración de Gunicorn y Nginx.
2. **Automatización**: Utilizar herramientas de automatización (como Ansible o Terraform) para gestionar la configuración y el despliegue.

#### Depuración y solución de problemas comunes

1. **Logs**: Revisar los logs de Gunicorn y Nginx para identificar y solucionar problemas.
2. **Errores comunes**: Solucionar errores comunes como `502 Bad Gateway` (generalmente causado por problemas de comunicación entre Nginx y Gunicorn) y `504 Gateway Timeout` (generalmente causado por tiempos de respuesta largos en Gunicorn).
3. **Monitoreo**: Utilizar herramientas de monitoreo (como Prometheus o Grafana) para supervisar el rendimiento y la salud del sistema.

En resumen, esta unidad proporciona una guía completa para configurar Gunicorn y Nginx para el despliegue de un sistema de carrito de compras desarrollado con Django. Al seguir estas instrucciones y aplicar las mejores prácticas, podrás asegurar que tu aplicación es segura, escalable y de alto rendimiento.

# Unidad: Uso de Docker y docker-compose

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en el uso de Docker y docker-compose para el despliegue y la gestión de un sistema de carrito de compras desarrollado con Django. Docker es una plataforma que permite empaquetar aplicaciones y sus dependencias en contenedores, asegurando que se ejecuten de manera consistente en cualquier entorno. Docker-compose, por otro lado, es una herramienta que facilita la definición y ejecución de aplicaciones multicontenedor.

### Objetivos de aprendizaje

1. Comprender los conceptos básicos de Docker y docker-compose.
2. Aprender a crear y configurar archivos Dockerfile y docker-compose.yml.
3. Desarrollar habilidades para construir, ejecutar y gestionar contenedores Docker.
4. Implementar un sistema de carrito de compras con Django utilizando Docker y docker-compose.
5. Aplicar mejores prácticas en el uso de Docker para el despliegue y la gestión de aplicaciones.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

El objetivo principal de esta unidad es desplegar un sistema de carrito de compras con Django utilizando Docker y docker-compose. La funcionalidad incluye:

1. **Creación de un Dockerfile**: Definir un archivo Dockerfile que contenga las instrucciones necesarias para construir una imagen Docker de la aplicación Django.
2. **Configuración de docker-compose**: Crear un archivo docker-compose.yml para definir y gestionar los servicios necesarios, como la base de datos y la aplicación web.
3. **Construcción y ejecución de contenedores**: Construir las imágenes Docker y ejecutar los contenedores utilizando docker-compose.
4. **Persistencia de datos**: Configurar volúmenes para asegurar la persistencia de los datos de la base de datos.
5. **Escalabilidad**: Configurar docker-compose para escalar los servicios según sea necesario.

### Casos de uso

#### Caso de uso 1: Construcción de la imagen Docker

**Actor**: Desarrollador

**Descripción**: El desarrollador crea un Dockerfile que especifica las instrucciones para construir una imagen Docker de la aplicación Django.

**Flujo de eventos**:
1. El desarrollador crea un archivo Dockerfile en el directorio raíz del proyecto Django.
2. El desarrollador define las instrucciones necesarias en el Dockerfile.
3. El desarrollador ejecuta el comando `docker build` para construir la imagen Docker.

#### Caso de uso 2: Configuración y ejecución de docker-compose

**Actor**: Desarrollador

**Descripción**: El desarrollador crea un archivo docker-compose.yml para definir y gestionar los servicios necesarios para la aplicación.

**Flujo de eventos**:
1. El desarrollador crea un archivo docker-compose.yml en el directorio raíz del proyecto.
2. El desarrollador define los servicios necesarios en el archivo docker-compose.yml.
3. El desarrollador ejecuta el comando `docker-compose up` para iniciar los servicios.

#### Caso de uso 3: Persistencia de datos

**Actor**: Desarrollador

**Descripción**: El desarrollador configura volúmenes en docker-compose.yml para asegurar la persistencia de los datos de la base de datos.

**Flujo de eventos**:
1. El desarrollador define volúmenes en el archivo docker-compose.yml.
2. El desarrollador ejecuta el comando `docker-compose up` para iniciar los servicios con volúmenes configurados.

### Diagramas de flujo

```mermaid
graph TD;
    A[Inicio] --> B[Crear Dockerfile]
    B --> C[Definir instrucciones en Dockerfile]
    C --> D[Ejecutar comando docker build]
    D --> E[Crear archivo docker-compose.yml]
    E --> F[Definir servicios en docker-compose.yml]
    F --> G[Ejecutar comando docker-compose up]
    G --> H[Configurar volúmenes en docker-compose.yml]
    H --> I[Ejecutar comando docker-compose up con volúmenes]
    I --> J[Fin]
```

### Requisitos no funcionales

1. **Rendimiento**: La construcción y ejecución de contenedores debe ser eficiente y rápida.
2. **Escalabilidad**: La configuración de docker-compose debe permitir escalar los servicios según sea necesario.
3. **Seguridad**: Asegurar que las imágenes Docker no contengan vulnerabilidades y que los datos sensibles estén protegidos.
4. **Mantenibilidad**: El Dockerfile y el archivo docker-compose.yml deben ser fáciles de entender y mantener.

## Implementación en Python

### Explicación paso a paso del código

#### Paso 1: Creación del Dockerfile

El Dockerfile es un archivo de texto que contiene las instrucciones para construir una imagen Docker. A continuación, se muestra un ejemplo de Dockerfile para una aplicación Django:

```Dockerfile
# Usar una imagen base oficial de Python
FROM python:3.9-slim

# Establecer el directorio de trabajo en el contenedor
WORKDIR /app

# Copiar los archivos de requerimientos al contenedor
COPY requirements.txt .

# Instalar las dependencias
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código de la aplicación al contenedor
COPY . .

# Exponer el puerto en el que la aplicación escuchará
EXPOSE 8000

# Definir el comando por defecto para ejecutar la aplicación
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "myproject.wsgi:application"]
```

#### Paso 2: Configuración de docker-compose

El archivo docker-compose.yml define los servicios necesarios para la aplicación. A continuación, se muestra un ejemplo de archivo docker-compose.yml:

```yaml
version: '3.8'

services:
  db:
    image: postgres:13
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: mydatabase
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword

  web:
    build: .
    command: gunicorn --bind 0.0.0.0:8000 myproject.wsgi:application
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  postgres_data:
```

#### Paso 3: Construcción y ejecución de contenedores

Para construir y ejecutar los contenedores, se utilizan los siguientes comandos:

```bash
# Construir la imagen Docker
docker build -t my-django-app .

# Ejecutar los servicios definidos en docker-compose.yml
docker-compose up
```

### Código fuente completo y comentado

A continuación, se presenta el código fuente completo y comentado para la implementación de Docker y docker-compose en la aplicación Django.

#### Dockerfile

```Dockerfile
# Usar una imagen base oficial de Python
FROM python:3.9-slim

# Establecer el directorio de trabajo en el contenedor
WORKDIR /app

# Copiar los archivos de requerimientos al contenedor
COPY requirements.txt .

# Instalar las dependencias
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código de la aplicación al contenedor
COPY . .

# Exponer el puerto en el que la aplicación escuchará
EXPOSE 8000

# Definir el comando por defecto para ejecutar la aplicación
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "myproject.wsgi:application"]
```

#### docker-compose.yml

```yaml
version: '3.8'

services:
  db:
    image: postgres:13
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: mydatabase
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword

  web:
    build: .
    command: gunicorn --bind 0.0.0.0:8000 myproject.wsgi:application
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  postgres_data:
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Para construir y ejecutar los contenedores, se deben seguir los siguientes pasos:

1. Crear el archivo Dockerfile y docker-compose.yml en el directorio raíz del proyecto Django.
2. Ejecutar el comando `docker-compose up` para iniciar los servicios.

```bash
# Construir la imagen Docker
docker build -t my-django-app .

# Ejecutar los servicios definidos en docker-compose.yml
docker-compose up
```

#### Pruebas unitarias

Para asegurar que la configuración de Docker y docker-compose funciona correctamente, se pueden realizar las siguientes pruebas:

1. **Prueba de construcción de la imagen Docker**: Ejecutar el comando `docker build` y verificar que la imagen se construya sin errores.
2. **Prueba de ejecución de contenedores**: Ejecutar el comando `docker-compose up` y verificar que los contenedores se inicien correctamente.
3. **Prueba de persistencia de datos**: Verificar que los datos de la base de datos se mantengan después de reiniciar los contenedores.

### Mejores prácticas y consideraciones de diseño

1. **Mantener el Dockerfile limpio y organizado**: Utilizar comentarios y dividir las instrucciones en pasos lógicos.
2. **Utilizar imágenes base ligeras**: Elegir imágenes base que sean ligeras y seguras para reducir el tamaño de la imagen Docker.
3. **Configurar volúmenes para la persistencia de datos**: Asegurar que los datos importantes se almacenen en volúmenes para evitar la pérdida de datos.
4. **Utilizar variables de entorno para la configuración**: Evitar hardcodear valores sensibles en el Dockerfile y docker-compose.yml. Utilizar variables de entorno para configurar la base de datos y otros servicios.
5. **Seguridad**: Asegurar que las imágenes Docker no contengan vulnerabilidades y que los datos sensibles estén protegidos. Utilizar herramientas como Docker Bench for Security para auditar la seguridad de las imágenes y contenedores.
6. **Automatización**: Integrar la construcción y despliegue de contenedores en el pipeline de CI/CD para asegurar una entrega continua y eficiente.

## Conclusión

En esta unidad, hemos explorado el uso de Docker y docker-compose para el despliegue y la gestión de un sistema de carrito de compras desarrollado con Django. Hemos aprendido a crear y configurar archivos Dockerfile y docker-compose.yml, construir y ejecutar contenedores, y aplicar mejores prácticas en el uso de Docker. Con estas habilidades, estamos mejor preparados para desplegar y gestionar aplicaciones de manera eficiente y escalable utilizando contenedores Docker.

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

# Unidad: Monitoreo con Django Debug Toolbar en desarrollo

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, exploraremos en profundidad el uso de Django Debug Toolbar para el monitoreo y depuración de aplicaciones Django durante el desarrollo. Este tema es parte del capítulo "Despliegue y DevOps" del libro "Sistema de Carrito de Compras con Django". La Django Debug Toolbar es una herramienta poderosa que permite a los desarrolladores obtener información detallada sobre las consultas SQL, las solicitudes HTTP, el tiempo de carga de las páginas y otros aspectos críticos de una aplicación Django. Al finalizar esta unidad, los lectores serán capaces de:

1. **Instalar y configurar Django Debug Toolbar en un proyecto Django.**
2. **Utilizar las funcionalidades de Django Debug Toolbar para identificar y resolver problemas de rendimiento y errores en el código.**
3. **Comprender los diferentes paneles y métricas que proporciona la herramienta.**
4. **Implementar mejores prácticas para el uso de herramientas de depuración en entornos de desarrollo.**

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

La Django Debug Toolbar es una herramienta de depuración que se integra con Django para proporcionar una interfaz visual que muestra información detallada sobre las solicitudes y respuestas HTTP, las consultas SQL ejecutadas, el tiempo de procesamiento de las vistas, y otros aspectos importantes del rendimiento de la aplicación. Esta herramienta es especialmente útil durante el desarrollo, ya que permite a los desarrolladores identificar y solucionar problemas de manera rápida y eficiente.

### Casos de uso

#### Caso de uso 1: Instalación y configuración de Django Debug Toolbar

**Actor:** Desarrollador

**Descripción:** El desarrollador instala y configura Django Debug Toolbar en un proyecto Django para comenzar a monitorear y depurar la aplicación.

**Precondiciones:**
- El proyecto Django está configurado y en funcionamiento.
- El entorno de desarrollo está correctamente configurado.

**Flujo de eventos:**
1. El desarrollador instala Django Debug Toolbar utilizando pip.
2. El desarrollador añade 'debug_toolbar' a la lista de aplicaciones instaladas en `settings.py`.
3. El desarrollador configura las URLs de la toolbar en `urls.py`.
4. El desarrollador ajusta la configuración de middleware y otras opciones necesarias.

**Postcondiciones:**
- Django Debug Toolbar está instalada y configurada correctamente.
- La toolbar aparece en las páginas de la aplicación durante el desarrollo.

#### Caso de uso 2: Monitoreo de consultas SQL

**Actor:** Desarrollador

**Descripción:** El desarrollador utiliza Django Debug Toolbar para monitorear y optimizar las consultas SQL ejecutadas por la aplicación.

**Precondiciones:**
- Django Debug Toolbar está instalada y configurada.
- La aplicación está en funcionamiento en el entorno de desarrollo.

**Flujo de eventos:**
1. El desarrollador navega a una página de la aplicación.
2. La toolbar muestra un panel con las consultas SQL ejecutadas.
3. El desarrollador revisa las consultas para identificar posibles problemas de rendimiento.
4. El desarrollador optimiza las consultas según sea necesario.

**Postcondiciones:**
- Las consultas SQL problemáticas son identificadas y optimizadas.
- El rendimiento de la aplicación mejora.

### Diagramas de flujo

```mermaid
graph TD
    A[Inicio] --> B[Instalar Django Debug Toolbar]
    B --> C[Añadir 'debug_toolbar' a INSTALLED_APPS]
    C --> D[Configurar middleware en settings.py]
    D --> E[Configurar URLs en urls.py]
    E --> F[Iniciar servidor de desarrollo]
    F --> G[Verificar aparición de la toolbar]
    G --> H[Monitorear consultas SQL]
    H --> I[Optimizar consultas]
    I --> J[Fin]
```

### Requisitos no funcionales

1. **Rendimiento:** La Django Debug Toolbar debe tener un impacto mínimo en el rendimiento del entorno de desarrollo.
2. **Usabilidad:** La interfaz de la toolbar debe ser intuitiva y fácil de usar para los desarrolladores.
3. **Compatibilidad:** La herramienta debe ser compatible con la versión de Django utilizada en el proyecto.
4. **Seguridad:** La toolbar debe estar deshabilitada en entornos de producción para evitar la exposición de información sensible.

## Implementación en Python

### Explicación paso a paso del código

#### Paso 1: Instalación de Django Debug Toolbar

Para instalar Django Debug Toolbar, utilizamos pip, el gestor de paquetes de Python. Ejecuta el siguiente comando en la terminal:

```bash
pip install django-debug-toolbar
```

#### Paso 2: Configuración en `settings.py`

Añade 'debug_toolbar' a la lista de aplicaciones instaladas en `settings.py`:

```python
INSTALLED_APPS = [
    ...
    'debug_toolbar',
    ...
]
```

Luego, añade el middleware de Django Debug Toolbar:

```python
MIDDLEWARE = [
    ...
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    ...
]
```

Configura las direcciones IP internas para permitir la visualización de la toolbar:

```python
INTERNAL_IPS = [
    '127.0.0.1',
]
```

#### Paso 3: Configuración de URLs en `urls.py`

Añade las URLs de Django Debug Toolbar en `urls.py`:

```python
from django.conf import settings
from django.conf.urls import include, url

if settings.DEBUG:
    import debug_toolbar
    urlpatterns = [
        url(r'^__debug__/', include(debug_toolbar.urls)),
    ] + urlpatterns
```

### Código fuente completo y comentado

```python
# settings.py

INSTALLED_APPS = [
    ...
    'debug_toolbar',  # Añadir la aplicación de Django Debug Toolbar
    ...
]

MIDDLEWARE = [
    ...
    'debug_toolbar.middleware.DebugToolbarMiddleware',  # Añadir el middleware de Django Debug Toolbar
    ...
]

INTERNAL_IPS = [
    '127.0.0.1',  # Configurar las IPs internas permitidas
]

# urls.py

from django.conf import settings
from django.conf.urls import include, url

urlpatterns = [
    ...
]

if settings.DEBUG:
    import debug_toolbar
    urlpatterns = [
        url(r'^__debug__/', include(debug_toolbar.urls)),  # Añadir las URLs de Django Debug Toolbar
    ] + urlpatterns
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso: Monitoreo de consultas SQL

1. Inicia el servidor de desarrollo:

```bash
python manage.py runserver
```

2. Navega a una página de la aplicación. La Django Debug Toolbar debería aparecer en el lado derecho de la página.
3. Haz clic en el panel "SQL" para ver las consultas SQL ejecutadas.
4. Revisa las consultas para identificar posibles problemas de rendimiento.

#### Pruebas unitarias

Para probar que Django Debug Toolbar está correctamente configurada, puedes crear una prueba unitaria que verifique la presencia de la toolbar en una página de la aplicación.

```python
from django.test import TestCase, Client
from django.conf import settings

class DebugToolbarTest(TestCase):
    def setUp(self):
        self.client = Client()

    def test_debug_toolbar_presence(self):
        if settings.DEBUG:
            response = self.client.get('/')
            self.assertContains(response, 'djDebug', status_code=200)
```

## Mejores prácticas y consideraciones de diseño

1. **Deshabilitar en producción:** Asegúrate de que Django Debug Toolbar esté deshabilitada en entornos de producción para evitar la exposición de información sensible.
2. **Optimización continua:** Utiliza la información proporcionada por la toolbar para identificar y solucionar problemas de rendimiento de manera continua durante el desarrollo.
3. **Documentación:** Mantén una documentación clara sobre cómo utilizar la toolbar y qué métricas observar para los nuevos miembros del equipo.
4. **Configuración personalizada:** Ajusta la configuración de Django Debug Toolbar según las necesidades específicas de tu proyecto para obtener la información más relevante.

### Consideraciones de seguridad

- **Acceso restringido:** Configura `INTERNAL_IPS` para restringir el acceso a la toolbar solo a direcciones IP internas.
- **Deshabilitar en producción:** Asegúrate de que la configuración de la toolbar esté condicionada por el entorno de desarrollo (`DEBUG = True`).

### Consideraciones de rendimiento

- **Impacto mínimo:** La toolbar debe tener un impacto mínimo en el rendimiento del entorno de desarrollo. Si notas una disminución significativa en el rendimiento, revisa la configuración y desactiva paneles innecesarios.

### Consejos para la depuración y solución de problemas comunes

1. **Toolbar no aparece:** Verifica que `DEBUG` esté configurado en `True` y que la dirección IP esté en `INTERNAL_IPS`.
2. **Problemas de compatibilidad:** Asegúrate de que la versión de Django Debug Toolbar sea compatible con la versión de Django utilizada en tu proyecto.
3. **Errores en consultas SQL:** Utiliza el panel "SQL" para identificar consultas lentas o ineficientes y optimízalas.

Con esta unidad, hemos cubierto en detalle cómo instalar, configurar y utilizar Django Debug Toolbar para monitorear y depurar aplicaciones Django durante el desarrollo. Esta herramienta es esencial para identificar y solucionar problemas de rendimiento, lo que resulta en una aplicación más eficiente y robusta.

# Unidad: Logging y Manejo de Errores en Producción

## 1. Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en el logging y el manejo de errores en un entorno de producción para un sistema de carrito de compras desarrollado con Django. El objetivo es proporcionar una comprensión profunda de cómo registrar eventos y manejar errores de manera efectiva para garantizar la estabilidad y la capacidad de mantenimiento del sistema en producción. Al final de esta unidad, los lectores deberían ser capaces de:

1. Comprender la importancia del logging y el manejo de errores en producción.
2. Configurar el logging en Django para registrar eventos importantes.
3. Implementar estrategias de manejo de errores para capturar y gestionar excepciones.
4. Utilizar herramientas y técnicas para monitorear y analizar los logs.
5. Aplicar mejores prácticas para el logging y el manejo de errores en un entorno de producción.

## 2. Documento Funcional de Requerimientos

### a. Descripción Detallada de la Funcionalidad

El logging y el manejo de errores son componentes críticos en cualquier aplicación de producción. En el contexto de un sistema de carrito de compras con Django, estas funcionalidades permiten registrar eventos importantes, detectar y gestionar errores, y proporcionar información valiosa para la resolución de problemas y la mejora continua del sistema.

#### Logging

El logging implica registrar eventos que ocurren durante la ejecución de la aplicación. Estos eventos pueden incluir información, advertencias, errores y eventos críticos. El objetivo es proporcionar un registro detallado de la actividad del sistema que pueda ser utilizado para monitorear el rendimiento, diagnosticar problemas y realizar auditorías.

#### Manejo de Errores

El manejo de errores implica capturar y gestionar excepciones que ocurren durante la ejecución de la aplicación. Esto incluye la detección de errores, la notificación a los desarrolladores o administradores del sistema, y la implementación de estrategias para mitigar el impacto de los errores en los usuarios finales.

### b. Casos de Uso

#### Caso de Uso 1: Registro de Eventos de Usuario

**Descripción:** El sistema debe registrar eventos importantes relacionados con las acciones de los usuarios, como iniciar sesión, agregar productos al carrito, realizar una compra, etc.

**Actores:** Usuario, Administrador del Sistema

**Flujo Principal:**
1. El usuario realiza una acción en el sistema (por ejemplo, inicia sesión).
2. El sistema registra el evento en el archivo de logs.
3. El administrador del sistema puede revisar los logs para monitorear la actividad del usuario.

#### Caso de Uso 2: Manejo de Errores en el Proceso de Pago

**Descripción:** El sistema debe capturar y gestionar errores que ocurren durante el proceso de pago, como errores de conexión con el proveedor de pagos o fallos en la validación de la tarjeta de crédito.

**Actores:** Usuario, Administrador del Sistema, Proveedor de Pagos

**Flujo Principal:**
1. El usuario intenta realizar un pago.
2. Ocurre un error durante el proceso de pago.
3. El sistema captura la excepción y registra el error en los logs.
4. El sistema notifica al usuario sobre el error y proporciona instrucciones para resolverlo.
5. El administrador del sistema revisa los logs para diagnosticar y solucionar el problema.

### c. Diagramas de Flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Usuario realiza una acción]
    B --> C[Registrar evento en el archivo de logs]
    C --> D[Fin]

    E[Inicio] --> F[Usuario intenta realizar un pago]
    F --> G{Ocurre un error?}
    G -->|Sí| H[Capturar excepción]
    H --> I[Registrar error en los logs]
    I --> J[Notificar al usuario]
    J --> K[Revisar logs para diagnosticar el problema]
    K --> L[Fin]
    G -->|No| L
```

### d. Requisitos No Funcionales

1. **Rendimiento:** El sistema de logging debe ser eficiente y no debe afectar significativamente el rendimiento de la aplicación.
2. **Escalabilidad:** El sistema debe ser capaz de manejar un gran volumen de logs y errores a medida que la aplicación escala.
3. **Seguridad:** Los logs deben ser almacenados de manera segura y deben ser accesibles solo para usuarios autorizados.
4. **Mantenibilidad:** El sistema de logging y manejo de errores debe ser fácil de mantener y actualizar.

## 3. Implementación en Python

### a. Explicación Paso a Paso del Código

Para implementar el logging y el manejo de errores en Django, seguiremos los siguientes pasos:

1. **Configurar el Logging en Django:**
   Django proporciona una configuración de logging basada en el módulo `logging` de Python. Configuraremos el logging en el archivo `settings.py`.

2. **Registrar Eventos Importantes:**
   Utilizaremos el módulo `logging` para registrar eventos importantes en diferentes partes de la aplicación.

3. **Capturar y Gestionar Excepciones:**
   Implementaremos middleware y manejadores de excepciones personalizados para capturar y gestionar errores.

4. **Monitorear y Analizar los Logs:**
   Utilizaremos herramientas como `Sentry` para monitorear y analizar los logs en producción.

### b. Código Fuente Completo y Comentado

#### Configuración del Logging en `settings.py`

```python
# settings.py

import os
import logging
import logging.config

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'verbose': {
            'format': '{levelname} {asctime} {module} {message}',
            'style': '{',
        },
        'simple': {
            'format': '{levelname} {message}',
            'style': '{',
        },
    },
    'handlers': {
        'file': {
            'level': 'DEBUG',
            'class': 'logging.FileHandler',
            'filename': os.path.join(BASE_DIR, 'logs/debug.log'),
            'formatter': 'verbose',
        },
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'simple',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file', 'console'],
            'level': 'DEBUG',
            'propagate': True,
        },
        'myapp': {
            'handlers': ['file', 'console'],
            'level': 'DEBUG',
            'propagate': False,
        },
    },
}
```

#### Registrar Eventos Importantes

```python
# views.py

import logging

logger = logging.getLogger('myapp')

def user_login(request):
    # Código para manejar el inicio de sesión del usuario
    logger.info(f'Usuario {request.user.username} ha iniciado sesión')
    # Resto del código

def add_to_cart(request, product_id):
    # Código para agregar un producto al carrito
    logger.info(f'Producto {product_id} agregado al carrito por el usuario {request.user.username}')
    # Resto del código

def process_payment(request):
    try:
        # Código para procesar el pago
        logger.info(f'Pago procesado para el usuario {request.user.username}')
    except Exception as e:
        logger.error(f'Error al procesar el pago para el usuario {request.user.username}: {str(e)}')
        # Manejo del error
```

#### Capturar y Gestionar Excepciones

```python
# middleware.py

import logging

logger = logging.getLogger('myapp')

class CustomExceptionMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        response = self.get_response(request)
        return response

    def process_exception(self, request, exception):
        logger.error(f'Excepción no manejada: {str(exception)}', exc_info=True)
        # Aquí podríamos redirigir a una página de error personalizada
        return None
```

#### Monitorear y Analizar los Logs con Sentry

Primero, instalamos Sentry:

```bash
pip install sentry-sdk
```

Luego, configuramos Sentry en `settings.py`:

```python
# settings.py

import sentry_sdk
from sentry_sdk.integrations.django import DjangoIntegration

sentry_sdk.init(
    dsn='YOUR_SENTRY_DSN',
    integrations=[DjangoIntegration()],
    traces_sample_rate=1.0,
    send_default_pii=True
)
```

### c. Ejemplos de Uso y Pruebas Unitarias

#### Ejemplo de Uso

```python
# Ejemplo de uso en una vista

def checkout(request):
    try:
        process_payment(request)
        return HttpResponse('Pago realizado con éxito')
    except Exception as e:
        logger.error(f'Error en el checkout: {str(e)}')
        return HttpResponse('Error al realizar el pago', status=500)
```

#### Pruebas Unitarias

```python
# tests.py

from django.test import TestCase
from django.urls import reverse
import logging

logger = logging.getLogger('myapp')

class LoggingTests(TestCase):
    def test_user_login_logging(self):
        response = self.client.post(reverse('login'), {'username': 'testuser', 'password': 'password'})
        self.assertEqual(response.status_code, 200)
        # Aquí podríamos verificar que el log se haya registrado correctamente

    def test_add_to_cart_logging(self):
        response = self.client.post(reverse('add_to_cart'), {'product_id': 1})
        self.assertEqual(response.status_code, 200)
        # Aquí podríamos verificar que el log se haya registrado correctamente

    def test_process_payment_logging(self):
        response = self.client.post(reverse('checkout'))
        self.assertEqual(response.status_code, 200)
        # Aquí podríamos verificar que el log se haya registrado correctamente
```

## 4. Mejores Prácticas y Consideraciones de Diseño

### Mejores Prácticas

1. **Registrar Eventos Importantes:** Asegúrate de registrar eventos importantes que puedan ayudar a diagnosticar problemas y monitorear la actividad del sistema.
2. **Usar Niveles de Logging Apropiados:** Utiliza niveles de logging apropiados (DEBUG, INFO, WARNING, ERROR, CRITICAL) para diferenciar la importancia de los eventos registrados.
3. **Proteger Información Sensible:** No registres información sensible como contraseñas o datos de tarjetas de crédito en los logs.
4. **Monitorear los Logs Regularmente:** Revisa los logs regularmente para detectar y solucionar problemas de manera proactiva.
5. **Configurar Alertas:** Configura alertas para eventos críticos que requieran atención inmediata.

### Consideraciones de Diseño

1. **Rendimiento:** Asegúrate de que el sistema de logging no afecte significativamente el rendimiento de la aplicación.
2. **Escalabilidad:** Diseña el sistema de logging para manejar un gran volumen de eventos a medida que la aplicación escala.
3. **Seguridad:** Protege los logs para evitar accesos no autorizados y asegúrate de que se almacenen de manera segura.
4. **Mantenibilidad:** Mantén la configuración de logging y manejo de errores bien documentada y fácil de mantener.

### Consejos para la Depuración y Solución de Problemas Comunes

1. **Verificar la Configuración de Logging:** Asegúrate de que la configuración de logging en `settings.py` esté correcta y que los handlers y loggers estén configurados adecuadamente.
2. **Revisar los Logs:** Revisa los logs para obtener pistas sobre la causa de los problemas. Los mensajes de error y las trazas de pila pueden proporcionar información valiosa.
3. **Utilizar Herramientas de Monitoreo:** Utiliza herramientas como Sentry para monitorear y analizar los logs en producción. Estas herramientas pueden proporcionar información adicional y alertas en tiempo real.
4. **Probar en un Entorno de Desarrollo:** Realiza pruebas exhaustivas en un entorno de desarrollo antes de desplegar cambios en producción. Esto puede ayudar a detectar y solucionar problemas antes de que afecten a los usuarios finales.

En resumen, el logging y el manejo de errores son componentes esenciales para garantizar la estabilidad y la capacidad de mantenimiento de un sistema de carrito de compras en producción. Al seguir las mejores prácticas y considerar aspectos de rendimiento, escalabilidad y seguridad, podemos implementar un sistema robusto y eficiente para registrar eventos y gestionar errores en Django.

# Unidad: Implementación de HTTPS

## 1. Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en la implementación de HTTPS en un sistema de carrito de compras desarrollado con Django. HTTPS (HyperText Transfer Protocol Secure) es una extensión de HTTP que utiliza SSL/TLS para cifrar la comunicación entre el cliente y el servidor, garantizando la integridad y confidencialidad de los datos transmitidos. Dado que nuestro sistema de carrito de compras maneja información sensible, como datos de usuarios y transacciones, es crucial asegurar que esta información esté protegida contra posibles ataques.

### Objetivos de aprendizaje

Al finalizar esta unidad, los lectores serán capaces de:

1. Comprender la importancia de HTTPS en aplicaciones web.
2. Configurar un certificado SSL/TLS en un servidor Django.
3. Implementar redirecciones automáticas de HTTP a HTTPS.
4. Verificar la correcta configuración y funcionamiento de HTTPS.
5. Aplicar mejores prácticas de seguridad en la implementación de HTTPS.

## 2. Documento funcional de requerimientos

### a. Descripción detallada de la funcionalidad

La funcionalidad principal de esta unidad es asegurar que todas las comunicaciones entre el cliente y el servidor del sistema de carrito de compras se realicen a través de HTTPS. Esto incluye la configuración de un certificado SSL/TLS, la redirección automática de HTTP a HTTPS y la verificación de la correcta implementación de HTTPS.

### b. Casos de uso

#### Caso de uso 1: Configuración del certificado SSL/TLS

**Actor:** Administrador del sistema

**Descripción:** El administrador del sistema configura un certificado SSL/TLS en el servidor Django para habilitar HTTPS.

**Precondiciones:** El servidor Django está configurado y en funcionamiento.

**Postcondiciones:** El servidor Django acepta conexiones HTTPS.

#### Caso de uso 2: Redirección automática de HTTP a HTTPS

**Actor:** Usuario final

**Descripción:** Cuando un usuario intenta acceder al sistema de carrito de compras a través de HTTP, es redirigido automáticamente a la versión HTTPS del sitio.

**Precondiciones:** El servidor Django está configurado para aceptar conexiones HTTPS.

**Postcondiciones:** Todas las solicitudes HTTP son redirigidas a HTTPS.

#### Caso de uso 3: Verificación de la configuración de HTTPS

**Actor:** Administrador del sistema

**Descripción:** El administrador del sistema verifica que todas las comunicaciones entre el cliente y el servidor se realicen a través de HTTPS.

**Precondiciones:** El servidor Django está configurado para aceptar conexiones HTTPS y redirigir automáticamente HTTP a HTTPS.

**Postcondiciones:** Todas las comunicaciones entre el cliente y el servidor están cifradas y se realizan a través de HTTPS.

### c. Diagramas de flujo (en mermaid)

```mermaid
graph TD
    A[Inicio] --> B[Configurar certificado SSL/TLS]
    B --> C[Configurar servidor Django para HTTPS]
    C --> D[Configurar redirección de HTTP a HTTPS]
    D --> E[Verificar configuración de HTTPS]
    E --> F[Fin]
```

### d. Requisitos no funcionales

1. **Seguridad:** Todas las comunicaciones entre el cliente y el servidor deben estar cifradas utilizando HTTPS.
2. **Rendimiento:** La implementación de HTTPS no debe introducir una latencia significativa en las comunicaciones.
3. **Escalabilidad:** La configuración de HTTPS debe ser capaz de manejar un gran número de conexiones simultáneas.
4. **Compatibilidad:** La implementación debe ser compatible con los navegadores web modernos y dispositivos móviles.
5. **Mantenibilidad:** La configuración de HTTPS debe ser fácil de mantener y actualizar.

## 3. Implementación en Python

### a. Explicación paso a paso del código

Para implementar HTTPS en nuestro sistema de carrito de compras con Django, seguiremos los siguientes pasos:

1. **Obtener un certificado SSL/TLS:** Podemos obtener un certificado SSL/TLS de una autoridad certificadora (CA) como Let's Encrypt.
2. **Configurar el servidor Django para HTTPS:** Modificaremos la configuración de Django para aceptar conexiones HTTPS.
3. **Configurar redirección de HTTP a HTTPS:** Aseguraremos que todas las solicitudes HTTP sean redirigidas a HTTPS.
4. **Verificar la configuración:** Comprobaremos que todas las comunicaciones se realicen a través de HTTPS.

### b. Código fuente completo y comentado

#### Paso 1: Obtener un certificado SSL/TLS

Para obtener un certificado SSL/TLS gratuito de Let's Encrypt, podemos utilizar Certbot. Certbot es una herramienta que automatiza el proceso de obtención y renovación de certificados SSL/TLS.

Primero, instalamos Certbot en nuestro servidor:

```bash
sudo apt-get update
sudo apt-get install certbot
```

Luego, obtenemos el certificado SSL/TLS:

```bash
sudo certbot certonly --standalone -d example.com -d www.example.com
```

#### Paso 2: Configurar el servidor Django para HTTPS

Modificamos la configuración de Django para aceptar conexiones HTTPS. En el archivo `settings.py`, añadimos las siguientes configuraciones:

```python
# settings.py

# Importamos las librerías necesarias
import os

# Configuramos la ruta del certificado y la clave privada
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
CERT_FILE = os.path.join(BASE_DIR, 'path/to/fullchain.pem')
KEY_FILE = os.path.join(BASE_DIR, 'path/to/privkey.pem')

# Configuramos Django para usar HTTPS
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

#### Paso 3: Configurar redirección de HTTP a HTTPS

Para redirigir automáticamente todas las solicitudes HTTP a HTTPS, podemos utilizar un middleware personalizado. Creamos un archivo llamado `middleware.py` en nuestra aplicación Django y añadimos el siguiente código:

```python
# middleware.py

from django.conf import settings
from django.http import HttpResponsePermanentRedirect

class SSLRedirectMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        if not request.is_secure() and not settings.DEBUG:
            url = request.build_absolute_uri(request.get_full_path())
            secure_url = url.replace("http://", "https://")
            return HttpResponsePermanentRedirect(secure_url)
        response = self.get_response(request)
        return response
```

Luego, añadimos el middleware a la lista de middlewares en `settings.py`:

```python
# settings.py

MIDDLEWARE = [
    # Otros middlewares
    'path.to.middleware.SSLRedirectMiddleware',
    # Otros middlewares
]
```

#### Paso 4: Verificar la configuración

Para verificar que todas las comunicaciones se realicen a través de HTTPS, podemos utilizar herramientas como `curl` o navegadores web. Por ejemplo, podemos ejecutar el siguiente comando `curl` para comprobar la redirección:

```bash
curl -I http://example.com
```

La respuesta debe ser una redirección 301 a la versión HTTPS del sitio:

```bash
HTTP/1.1 301 Moved Permanently
Location: https://example.com/
```

### c. Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Supongamos que hemos configurado nuestro servidor Django para HTTPS y hemos implementado la redirección automática de HTTP a HTTPS. Ahora, cuando un usuario intenta acceder al sistema de carrito de compras a través de HTTP, será redirigido automáticamente a la versión HTTPS del sitio.

#### Pruebas unitarias

Podemos escribir pruebas unitarias para verificar que la redirección de HTTP a HTTPS funciona correctamente. Utilizamos el framework de pruebas de Django para escribir nuestras pruebas.

```python
# tests.py

from django.test import TestCase, Client

class SSLRedirectMiddlewareTest(TestCase):
    def setUp(self):
        self.client = Client()

    def test_http_to_https_redirect(self):
        response = self.client.get('http://example.com')
        self.assertEqual(response.status_code, 301)
        self.assertTrue(response['Location'].startswith('https://'))
```

### d. Mejores prácticas y consideraciones de diseño

1. **Renovación automática del certificado:** Utiliza Certbot para configurar la renovación automática del certificado SSL/TLS, asegurando que tu sitio web siempre tenga un certificado válido.
2. **Configuración de HSTS:** Habilita HTTP Strict Transport Security (HSTS) para indicar a los navegadores que solo deben comunicarse con tu sitio a través de HTTPS.
3. **Seguridad adicional:** Configura encabezados de seguridad adicionales, como `Content-Security-Policy` y `X-Frame-Options`, para proteger tu sitio contra ataques comunes.
4. **Monitoreo y alertas:** Implementa monitoreo y alertas para detectar y responder rápidamente a cualquier problema relacionado con HTTPS.
5. **Pruebas exhaustivas:** Realiza pruebas exhaustivas para asegurarte de que todas las rutas y recursos de tu sitio están accesibles a través de HTTPS.

## Conclusión

En esta unidad, hemos aprendido a implementar HTTPS en un sistema de carrito de compras desarrollado con Django. Hemos cubierto la obtención de un certificado SSL/TLS, la configuración del servidor Django para HTTPS, la redirección automática de HTTP a HTTPS y la verificación de la configuración. Además, hemos discutido las mejores prácticas y consideraciones de diseño para asegurar que nuestra implementación de HTTPS sea segura y eficiente. Con estos conocimientos, estamos mejor preparados para proteger la información sensible de nuestros usuarios y garantizar la seguridad de nuestro sistema de carrito de compras.

### Unidad: Protección contra CSRF, XSS y SQL Injection

#### Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, profundizaremos en las técnicas avanzadas para proteger un sistema de carrito de compras desarrollado con Django contra tres de las amenazas más comunes en aplicaciones web: Cross-Site Request Forgery (CSRF), Cross-Site Scripting (XSS) y SQL Injection. Aunque ya se han abordado estos temas en unidades anteriores, aquí exploraremos estrategias más avanzadas y específicas para Django, con el objetivo de garantizar un sistema robusto y seguro.

**Objetivos de Aprendizaje:**

1. Comprender en profundidad las amenazas de CSRF, XSS y SQL Injection.
2. Implementar medidas avanzadas de protección contra estas amenazas en un sistema de carrito de compras con Django.
3. Aprender a utilizar las herramientas y librerías de Django para mejorar la seguridad de la aplicación.
4. Realizar pruebas de seguridad para verificar la efectividad de las medidas implementadas.

#### Documento Funcional de Requerimientos

##### a. Descripción Detallada de la Funcionalidad

La funcionalidad principal de esta unidad es asegurar que el sistema de carrito de compras sea resistente a ataques de CSRF, XSS y SQL Injection. Esto se logrará mediante la implementación de prácticas de codificación segura y el uso de herramientas proporcionadas por Django.

**CSRF (Cross-Site Request Forgery):**
- Implementar tokens CSRF en formularios y solicitudes AJAX.
- Verificar la validez de los tokens en el servidor.

**XSS (Cross-Site Scripting):**
- Escapar y sanitizar todas las entradas de usuario.
- Utilizar plantillas seguras en Django para evitar la inserción de scripts maliciosos.

**SQL Injection:**
- Utilizar el ORM de Django para todas las interacciones con la base de datos.
- Evitar la construcción de consultas SQL dinámicas con entradas de usuario.

##### b. Casos de Uso

**Caso de Uso 1: Protección contra CSRF en Formularios**
- **Actor:** Usuario
- **Descripción:** Un usuario envía un formulario de compra en el sistema de carrito de compras.
- **Precondición:** El usuario debe estar autenticado.
- **Postcondición:** El servidor valida el token CSRF y procesa la solicitud si es válida.

**Caso de Uso 2: Protección contra XSS en Comentarios de Productos**
- **Actor:** Usuario
- **Descripción:** Un usuario publica un comentario en la página de un producto.
- **Precondición:** El usuario debe estar autenticado.
- **Postcondición:** El comentario se guarda en la base de datos después de ser sanitizado.

**Caso de Uso 3: Protección contra SQL Injection en Búsquedas de Productos**
- **Actor:** Usuario
- **Descripción:** Un usuario realiza una búsqueda de productos en el sistema.
- **Precondición:** El usuario debe estar autenticado.
- **Postcondición:** La consulta se ejecuta de manera segura utilizando el ORM de Django.

##### c. Diagramas de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Usuario envía formulario]
    B --> C[Servidor recibe solicitud]
    C --> D[Validar token CSRF]
    D -->|Token válido| E[Procesar solicitud]
    D -->|Token inválido| F[Rechazar solicitud]

    G[Inicio] --> H[Usuario publica comentario]
    H --> I[Sanitizar entrada]
    I --> J[Guardar comentario en DB]

    K[Inicio] --> L[Usuario realiza búsqueda]
    L --> M[Construir consulta con ORM]
    M --> N[Ejecutar consulta]
```

##### d. Requisitos No Funcionales

- **Seguridad:** La aplicación debe ser resistente a ataques CSRF, XSS y SQL Injection.
- **Rendimiento:** Las medidas de seguridad no deben afectar significativamente el rendimiento de la aplicación.
- **Escalabilidad:** La aplicación debe ser capaz de manejar un aumento en el número de usuarios sin comprometer la seguridad.
- **Mantenibilidad:** El código debe ser claro y bien documentado para facilitar futuras actualizaciones y mantenimientos.

#### Implementación en Python

##### a. Explicación Paso a Paso del Código

**Protección contra CSRF:**

Django proporciona protección contra CSRF de manera predeterminada. Sin embargo, es importante asegurarse de que todos los formularios y solicitudes AJAX incluyan el token CSRF.

1. **Incluir el Middleware de CSRF:**
   Django incluye `CsrfViewMiddleware` por defecto. Asegúrate de que esté habilitado en `MIDDLEWARE` en `settings.py`.

2. **Incluir el Token CSRF en Formularios:**
   En las plantillas de Django, utiliza `{% csrf_token %}` dentro de los formularios para incluir el token CSRF.

   ```html
   <form method="post" action="/comprar/">
       {% csrf_token %}
       <!-- otros campos del formulario -->
       <button type="submit">Comprar</button>
   </form>
   ```

3. **Incluir el Token CSRF en Solicitudes AJAX:**
   Para solicitudes AJAX, el token CSRF debe ser incluido en los encabezados de la solicitud.

   ```javascript
   function getCookie(name) {
       var cookieValue = null;
       if (document.cookie && document.cookie !== '') {
           var cookies = document.cookie.split(';');
           for (var i = 0; i < cookies.length; i++) {
               var cookie = cookies[i].trim();
               if (cookie.substring(0, name.length + 1) === (name + '=')) {
                   cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                   break;
               }
           }
       }
       return cookieValue;
   }
   var csrftoken = getCookie('csrftoken');

   function csrfSafeMethod(method) {
       return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
   }

   $.ajaxSetup({
       beforeSend: function(xhr, settings) {
           if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
               xhr.setRequestHeader("X-CSRFToken", csrftoken);
           }
       }
   });
   ```

**Protección contra XSS:**

1. **Escapar y Sanitizar Entradas de Usuario:**
   Utiliza las funciones de Django para escapar y sanitizar entradas de usuario antes de mostrarlas en las plantillas.

   ```python
   from django.utils.html import escape

   def publicar_comentario(request):
       if request.method == "POST":
           comentario = escape(request.POST.get('comentario'))
           # Guardar el comentario en la base de datos
   ```

2. **Utilizar Plantillas Seguras:**
   Django automáticamente escapa las variables en las plantillas. Asegúrate de no desactivar esta funcionalidad.

   ```html
   <p>{{ comentario }}</p>
   ```

**Protección contra SQL Injection:**

1. **Utilizar el ORM de Django:**
   Evita construir consultas SQL dinámicas con entradas de usuario. Utiliza el ORM de Django para todas las interacciones con la base de datos.

   ```python
   from .models import Producto

   def buscar_productos(request):
       query = request.GET.get('q')
       productos = Producto.objects.filter(nombre__icontains=query)
       return render(request, 'resultados.html', {'productos': productos})
   ```

##### b. Código Fuente Completo y Comentado

```python
# views.py

from django.shortcuts import render, redirect
from django.utils.html import escape
from .models import Producto, Comentario

def comprar_producto(request):
    if request.method == "POST":
        # Validar token CSRF
        # Django lo hace automáticamente si CSRF middleware está habilitado
        # Procesar la compra
        return redirect('compra_exitosa')
    return render(request, 'comprar.html')

def publicar_comentario(request):
    if request.method == "POST":
        comentario = escape(request.POST.get('comentario'))
        nuevo_comentario = Comentario(texto=comentario, usuario=request.user)
        nuevo_comentario.save()
        return redirect('producto_detalle', id=nuevo_comentario.producto.id)
    return render(request, 'publicar_comentario.html')

def buscar_productos(request):
    query = request.GET.get('q')
    productos = Producto.objects.filter(nombre__icontains=query)
    return render(request, 'resultados.html', {'productos': productos})
```

##### c. Ejemplos de Uso y Pruebas Unitarias

**Ejemplo de Uso:**

1. **Comprar Producto:**
   - El usuario accede a la página de compra, llena el formulario y envía la solicitud.
   - El servidor valida el token CSRF y procesa la compra.

2. **Publicar Comentario:**
   - El usuario publica un comentario en la página de un producto.
   - El comentario se sanitiza y se guarda en la base de datos.

3. **Buscar Productos:**
   - El usuario realiza una búsqueda de productos.
   - La consulta se ejecuta de manera segura utilizando el ORM de Django.

**Pruebas Unitarias:**

```python
# tests.py

from django.test import TestCase, Client
from django.urls import reverse
from .models import Producto, Comentario
from django.contrib.auth.models import User

class SeguridadTestCase(TestCase):
    def setUp(self):
        self.client = Client()
        self.user = User.objects.create_user(username='testuser', password='12345')
        self.client.login(username='testuser', password='12345')
        self.producto = Producto.objects.create(nombre='Producto Test', precio=100)

    def test_csrf_proteccion(self):
        response = self.client.post(reverse('comprar_producto'), {'producto_id': self.producto.id})
        self.assertEqual(response.status_code, 403)  # CSRF token missing or incorrect

    def test_xss_proteccion(self):
        comentario = '<script>alert("XSS")</script>'
        response = self.client.post(reverse('publicar_comentario'), {'comentario': comentario})
        self.assertNotIn(comentario, response.content.decode())
        self.assertIn('&lt;script&gt;alert(&quot;XSS&quot;)&lt;/script&gt;', response.content.decode())

    def test_sql_injection_proteccion(self):
        query = 'Producto Test" OR "1"="1'
        response = self.client.get(reverse('buscar_productos'), {'q': query})
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Producto Test')
```

#### Mejores Prácticas y Consideraciones de Diseño

1. **Mantener el Middleware de CSRF Habilitado:**
   Asegúrate de que el middleware de CSRF esté siempre habilitado en tu configuración de Django.

2. **Sanitizar Todas las Entradas de Usuario:**
   Nunca confíes en las entradas de usuario. Siempre escapa y sanitiza antes de procesarlas o almacenarlas.

3. **Utilizar el ORM de Django:**
   El ORM de Django no solo facilita las consultas a la base de datos, sino que también protege contra inyecciones SQL.

4. **Revisar y Actualizar Regularmente:**
   La seguridad es un proceso continuo. Revisa y actualiza regularmente tu código y dependencias para protegerte contra nuevas vulnerabilidades.

5. **Realizar Pruebas de Seguridad:**
   Implementa pruebas de seguridad automatizadas como parte de tu proceso de desarrollo para detectar y corregir vulnerabilidades de manera temprana.

6. **Consideraciones de Rendimiento:**
   Asegúrate de que las medidas de seguridad no afecten significativamente el rendimiento de la aplicación. Realiza pruebas de carga para identificar posibles cuellos de botella.

7. **Documentación y Mantenibilidad:**
   Documenta todas las medidas de seguridad implementadas y asegúrate de que el código sea claro y fácil de mantener.

En resumen, la protección contra CSRF, XSS y SQL Injection es crucial para la seguridad de cualquier aplicación web. Al seguir las mejores prácticas y utilizar las herramientas proporcionadas por Django, puedes crear un sistema de carrito de compras seguro y robusto.

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

# Unidad: Cumplimiento GDPR con django-gdpr-assist

## Introducción a la unidad y objetivos de aprendizaje

En esta unidad, nos enfocaremos en el cumplimiento del Reglamento General de Protección de Datos (GDPR) utilizando la librería `django-gdpr-assist` en el contexto de un sistema de carrito de compras desarrollado con Django. El GDPR es una regulación de la Unión Europea que establece directrices estrictas sobre la recopilación, almacenamiento y procesamiento de datos personales. Cumplir con estas regulaciones es crucial para evitar sanciones legales y mantener la confianza de los usuarios.

### Objetivos de aprendizaje

1. Comprender los principios fundamentales del GDPR y su aplicación en un sistema de carrito de compras.
2. Aprender a integrar y utilizar la librería `django-gdpr-assist` para facilitar el cumplimiento del GDPR.
3. Implementar funcionalidades específicas como la anonimización y eliminación de datos personales.
4. Realizar pruebas para asegurar que las implementaciones cumplen con los requisitos del GDPR.
5. Adoptar mejores prácticas y consideraciones de diseño para mantener la seguridad y el cumplimiento a largo plazo.

## Documento funcional de requerimientos

### Descripción detallada de la funcionalidad

El objetivo principal es asegurar que nuestro sistema de carrito de compras cumple con las regulaciones del GDPR. Esto incluye:

1. **Anonimización de datos**: Proceso mediante el cual los datos personales se transforman de manera que no se pueda identificar a una persona específica.
2. **Eliminación de datos**: Capacidad de eliminar completamente los datos personales de un usuario a petición.
3. **Portabilidad de datos**: Permitir a los usuarios exportar sus datos personales en un formato estructurado y comúnmente utilizado.
4. **Consentimiento informado**: Asegurar que los usuarios den su consentimiento explícito para la recopilación y procesamiento de sus datos personales.

### Casos de uso

1. **Anonimización de datos personales**:
   - **Actor**: Administrador del sistema.
   - **Descripción**: El administrador puede anonimizar los datos personales de usuarios inactivos para cumplir con las políticas de retención de datos.
   - **Precondiciones**: El usuario debe estar inactivo durante un período específico.
   - **Postcondiciones**: Los datos personales del usuario se anonimizarán y no se podrán utilizar para identificar al usuario.

2. **Eliminación de datos personales**:
   - **Actor**: Usuario registrado.
   - **Descripción**: Un usuario puede solicitar la eliminación completa de sus datos personales.
   - **Precondiciones**: El usuario debe autenticarse y confirmar su solicitud.
   - **Postcondiciones**: Todos los datos personales del usuario se eliminarán del sistema.

3. **Exportación de datos personales**:
   - **Actor**: Usuario registrado.
   - **Descripción**: Un usuario puede solicitar una copia de sus datos personales en un formato estructurado.
   - **Precondiciones**: El usuario debe autenticarse y solicitar la exportación.
   - **Postcondiciones**: El usuario recibirá un archivo con sus datos personales.

4. **Consentimiento informado**:
   - **Actor**: Usuario visitante.
   - **Descripción**: El sistema debe obtener el consentimiento explícito del usuario antes de recopilar y procesar sus datos personales.
   - **Precondiciones**: El usuario visita el sitio web por primera vez o después de que se hayan realizado cambios en la política de privacidad.
   - **Postcondiciones**: El consentimiento del usuario se almacena y se utiliza para futuras referencias.

### Diagramas de flujo (en mermaid)

```mermaid
flowchart TD
    A[Inicio] --> B[Usuario solicita eliminación de datos]
    B --> C{¿Usuario autenticado?}
    C -- No --> D[Solicitar autenticación]
    C -- Sí --> E[Confirmar solicitud]
    E --> F[Eliminar datos personales]
    F --> G[Notificar al usuario]
    G --> H[Fin]
```

```mermaid
flowchart TD
    A[Inicio] --> B[Usuario solicita exportación de datos]
    B --> C{¿Usuario autenticado?}
    C -- No --> D[Solicitar autenticación]
    C -- Sí --> E[Generar archivo de datos]
    E --> F[Enviar archivo al usuario]
    F --> G[Fin]
```

### Requisitos no funcionales

1. **Seguridad**: Los datos personales deben ser manejados de manera segura, utilizando cifrado tanto en tránsito como en reposo.
2. **Rendimiento**: Las operaciones de anonimización y eliminación de datos deben ser eficientes y no afectar el rendimiento general del sistema.
3. **Escalabilidad**: La solución debe ser escalable para manejar un gran número de solicitudes de anonimización y eliminación de datos.
4. **Auditoría**: Debe haber un registro de todas las operaciones de anonimización y eliminación de datos para fines de auditoría.
5. **Usabilidad**: La interfaz para solicitar la eliminación y exportación de datos debe ser intuitiva y fácil de usar.

## Implementación en Python

### Explicación paso a paso del código

Para implementar el cumplimiento del GDPR en nuestro sistema de carrito de compras, utilizaremos la librería `django-gdpr-assist`. Esta librería proporciona herramientas para anonimizar y eliminar datos personales de manera sencilla.

#### Paso 1: Instalación de django-gdpr-assist

Primero, necesitamos instalar la librería `django-gdpr-assist`:

```bash
pip install django-gdpr-assist
```

Luego, agregamos `gdpr_assist` a la lista de aplicaciones instaladas en `settings.py`:

```python
INSTALLED_APPS = [
    ...
    'gdpr_assist',
    ...
]
```

#### Paso 2: Configuración de modelos para GDPR

Para que `django-gdpr-assist` pueda manejar los datos personales, debemos configurar nuestros modelos. Supongamos que tenemos un modelo `Usuario` que almacena datos personales:

```python
from django.db import models
from gdpr_assist.models import PrivacyMeta

class Usuario(models.Model):
    nombre = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    direccion = models.CharField(max_length=255)
    telefono = models.CharField(max_length=15)
    fecha_registro = models.DateTimeField(auto_now_add=True)

    class Meta:
        privacy_meta = PrivacyMeta(
            fields = [
                'nombre',
                'email',
                'direccion',
                'telefono'
            ],
            search_fields = [
                'email'
            ]
        )
```

#### Paso 3: Anonimización de datos

Para anonimizar los datos personales de un usuario, podemos utilizar el siguiente método:

```python
from gdpr_assist.models import PrivacyManager

def anonimizar_usuario(usuario_id):
    try:
        usuario = Usuario.objects.get(id=usuario_id)
        usuario.anonimizar()
        usuario.save()
        return True
    except Usuario.DoesNotExist:
        return False
```

#### Paso 4: Eliminación de datos

Para eliminar los datos personales de un usuario, podemos definir el siguiente método:

```python
def eliminar_usuario(usuario_id):
    try:
        usuario = Usuario.objects.get(id=usuario_id)
        usuario.delete()
        return True
    except Usuario.DoesNotExist:
        return False
```

#### Paso 5: Exportación de datos

Para permitir la exportación de datos personales, podemos definir un método que genere un archivo JSON con los datos del usuario:

```python
import json
from django.http import JsonResponse

def exportar_datos_usuario(usuario_id):
    try:
        usuario = Usuario.objects.get(id=usuario_id)
        datos = {
            'nombre': usuario.nombre,
            'email': usuario.email,
            'direccion': usuario.direccion,
            'telefono': usuario.telefono,
            'fecha_registro': usuario.fecha_registro.isoformat()
        }
        return JsonResponse(datos)
    except Usuario.DoesNotExist:
        return JsonResponse({'error': 'Usuario no encontrado'}, status=404)
```

### Código fuente completo y comentado

```python
from django.db import models
from gdpr_assist.models import PrivacyMeta

class Usuario(models.Model):
    nombre = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    direccion = models.CharField(max_length=255)
    telefono = models.CharField(max_length=15)
    fecha_registro = models.DateTimeField(auto_now_add=True)

    class Meta:
        privacy_meta = PrivacyMeta(
            fields = [
                'nombre',
                'email',
                'direccion',
                'telefono'
            ],
            search_fields = [
                'email'
            ]
        )

# Función para anonimizar los datos de un usuario
def anonimizar_usuario(usuario_id):
    try:
        usuario = Usuario.objects.get(id=usuario_id)
        usuario.anonimizar()
        usuario.save()
        return True
    except Usuario.DoesNotExist:
        return False

# Función para eliminar los datos de un usuario
def eliminar_usuario(usuario_id):
    try:
        usuario = Usuario.objects.get(id=usuario_id)
        usuario.delete()
        return True
    except Usuario.DoesNotExist:
        return False

# Función para exportar los datos de un usuario en formato JSON
import json
from django.http import JsonResponse

def exportar_datos_usuario(usuario_id):
    try:
        usuario = Usuario.objects.get(id=usuario_id)
        datos = {
            'nombre': usuario.nombre,
            'email': usuario.email,
            'direccion': usuario.direccion,
            'telefono': usuario.telefono,
            'fecha_registro': usuario.fecha_registro.isoformat()
        }
        return JsonResponse(datos)
    except Usuario.DoesNotExist:
        return JsonResponse({'error': 'Usuario no encontrado'}, status=404)
```

### Ejemplos de uso y pruebas unitarias

#### Ejemplo de uso

Supongamos que queremos anonimizar los datos de un usuario con ID 1. Podemos llamar a la función `anonimizar_usuario` de la siguiente manera:

```python
resultado = anonimizar_usuario(1)
if resultado:
    print("Datos anonimizados exitosamente.")
else:
    print("Usuario no encontrado.")
```

#### Pruebas unitarias

Para asegurar que nuestras funciones funcionan correctamente, podemos escribir pruebas unitarias utilizando `unittest`:

```python
import unittest
from django.test import TestCase
from .models import Usuario, anonimizar_usuario, eliminar_usuario, exportar_datos_usuario

class GDPRTests(TestCase):

    def setUp(self):
        self.usuario = Usuario.objects.create(
            nombre="Juan Perez",
            email="juan.perez@example.com",
            direccion="Calle Falsa 123",
            telefono="123456789",
            fecha_registro="2023-01-01T00:00:00Z"
        )

    def test_anonimizar_usuario(self):
        resultado = anonimizar_usuario(self.usuario.id)
        self.assertTrue(resultado)
        usuario = Usuario.objects.get(id=self.usuario.id)
        self.assertEqual(usuario.nombre, "")
        self.assertEqual(usuario.email, "")
        self.assertEqual(usuario.direccion, "")
        self.assertEqual(usuario.telefono, "")

    def test_eliminar_usuario(self):
        resultado = eliminar_usuario(self.usuario.id)
        self.assertTrue(resultado)
        with self.assertRaises(Usuario.DoesNotExist):
            Usuario.objects.get(id=self.usuario.id)

    def test_exportar_datos_usuario(self):
        response = exportar_datos_usuario(self.usuario.id)
        self.assertEqual(response.status_code, 200)
        datos = response.json()
        self.assertEqual(datos['nombre'], "Juan Perez")
        self.assertEqual(datos['email'], "juan.perez@example.com")
        self.assertEqual(datos['direccion'], "Calle Falsa 123")
        self.assertEqual(datos['telefono'], "123456789")
        self.assertEqual(datos['fecha_registro'], "2023-01-01T00:00:00Z")
```

## Mejores prácticas y consideraciones de diseño

### Seguridad

1. **Cifrado de datos**: Asegúrate de que todos los datos personales estén cifrados tanto en tránsito como en reposo.
2. **Autenticación y autorización**: Implementa mecanismos robustos de autenticación y autorización para asegurar que solo los usuarios autorizados puedan acceder y modificar datos personales.
3. **Registro de auditoría**: Mantén un registro de todas las operaciones de anonimización y eliminación de datos para fines de auditoría y cumplimiento.

### Rendimiento

1. **Optimización de consultas**: Asegúrate de que las consultas a la base de datos estén optimizadas para minimizar el impacto en el rendimiento.
2. **Procesamiento en segundo plano**: Considera utilizar colas de tareas para manejar operaciones de anonimización y eliminación de datos en segundo plano, especialmente si se espera un alto volumen de solicitudes.

### Escalabilidad

1. **Diseño modular**: Diseña tu sistema de manera modular para facilitar la escalabilidad y el mantenimiento.
2. **Uso de caché**: Implementa mecanismos de caché para mejorar el rendimiento y reducir la carga en la base de datos.

### Usabilidad

1. **Interfaz intuitiva**: Asegúrate de que la interfaz para solicitar la eliminación y exportación de datos sea intuitiva y fácil de usar.
2. **Mensajes claros**: Proporciona mensajes claros y concisos para guiar a los usuarios a través del proceso de solicitud de eliminación y exportación de datos.

### Depuración y solución de problemas comunes

1. **Manejo de excepciones**: Implementa un manejo adecuado de excepciones para capturar y registrar errores.
2. **Pruebas exhaustivas**: Realiza pruebas exhaustivas para asegurar que todas las funcionalidades cumplen con los requisitos del GDPR.
3. **Documentación**: Mantén una documentación clara y actualizada para facilitar la comprensión y el mantenimiento del sistema.

En resumen, el cumplimiento del GDPR es un aspecto crucial en el desarrollo de un sistema de carrito de compras. Utilizando `django-gdpr-assist`, podemos implementar funcionalidades clave como la anonimización, eliminación y exportación de datos personales de manera eficiente y segura. Siguiendo las mejores prácticas y consideraciones de diseño, podemos asegurar que nuestro sistema no solo cumple con las regulaciones, sino que también ofrece una experiencia segura y confiable para los usuarios.

# Unidad: Auditoría de Seguridad y Penetration Testing

## Introducción a la Unidad y Objetivos de Aprendizaje

En esta unidad, nos enfocaremos en la auditoría de seguridad y el penetration testing (pruebas de penetración) en el contexto de un sistema de carrito de compras desarrollado con Django. La seguridad es un aspecto crítico en cualquier aplicación web, especialmente en aquellas que manejan información sensible como datos de usuarios y transacciones financieras. A través de esta unidad, aprenderás a identificar y mitigar vulnerabilidades en tu aplicación, asegurando que cumpla con los más altos estándares de seguridad.

### Objetivos de Aprendizaje
1. Comprender los conceptos fundamentales de auditoría de seguridad y penetration testing.
2. Aprender a realizar una auditoría de seguridad en una aplicación Django.
3. Implementar pruebas de penetración para identificar vulnerabilidades en el sistema de carrito de compras.
4. Aplicar mejores prácticas de seguridad y considerar aspectos de rendimiento y escalabilidad.
5. Desarrollar habilidades para la depuración y solución de problemas comunes en el contexto de la seguridad.

## Documento Funcional de Requerimientos

### Descripción Detallada de la Funcionalidad

La auditoría de seguridad y las pruebas de penetración son procesos esenciales para garantizar que una aplicación web sea segura y resistente a ataques maliciosos. En el contexto de nuestro sistema de carrito de compras con Django, estas prácticas nos permitirán identificar y corregir vulnerabilidades antes de que puedan ser explotadas por atacantes.

#### Auditoría de Seguridad
La auditoría de seguridad implica una revisión exhaustiva del código fuente, configuraciones y dependencias de la aplicación para identificar posibles vulnerabilidades. Este proceso puede incluir:
- Revisión de permisos y autenticación.
- Evaluación de la configuración del servidor y la base de datos.
- Análisis de dependencias y bibliotecas externas.
- Revisión de la implementación de medidas de seguridad como HTTPS, protección contra CSRF, XSS y SQL Injection.

#### Penetration Testing
El penetration testing, o pruebas de penetración, consiste en simular ataques reales contra la aplicación para identificar vulnerabilidades que podrían ser explotadas. Este proceso puede incluir:
- Pruebas de autenticación y autorización.
- Pruebas de inyección de código.
- Pruebas de seguridad en la transmisión de datos.
- Pruebas de configuración y despliegue.

### Casos de Uso

#### Caso de Uso 1: Auditoría de Seguridad
**Actor:** Auditor de Seguridad
**Descripción:** El auditor de seguridad revisa el código fuente, configuraciones y dependencias de la aplicación para identificar vulnerabilidades.
**Precondiciones:** El auditor tiene acceso al repositorio de código y a las configuraciones del servidor.
**Postcondiciones:** Se genera un informe detallado con las vulnerabilidades encontradas y recomendaciones para corregirlas.

#### Caso de Uso 2: Penetration Testing
**Actor:** Penetration Tester
**Descripción:** El penetration tester realiza pruebas de penetración en la aplicación para identificar vulnerabilidades explotables.
**Precondiciones:** El tester tiene acceso a un entorno de prueba que replica el entorno de producción.
**Postcondiciones:** Se genera un informe detallado con las vulnerabilidades encontradas y recomendaciones para mitigarlas.

### Diagramas de Flujo

```mermaid
flowchart TD
    A[Inicio] --> B[Auditoría de Seguridad]
    B --> C[Revisión de Código Fuente]
    C --> D[Evaluación de Configuraciones]
    D --> E[Análisis de Dependencias]
    E --> F[Generación de Informe]
    F --> G[Corrección de Vulnerabilidades]
    G --> H[Fin]

    A --> I[Penetration Testing]
    I --> J[Pruebas de Autenticación]
    J --> K[Pruebas de Inyección de Código]
    K --> L[Pruebas de Seguridad en Transmisión de Datos]
    L --> M[Pruebas de Configuración y Despliegue]
    M --> N[Generación de Informe]
    N --> O[Corrección de Vulnerabilidades]
    O --> H
```

### Requisitos No Funcionales

1. **Seguridad:** La aplicación debe ser resistente a ataques comunes como inyección de código, XSS, CSRF y otros.
2. **Rendimiento:** Las auditorías y pruebas de penetración no deben afectar significativamente el rendimiento de la aplicación en producción.
3. **Escalabilidad:** Las prácticas de seguridad implementadas deben ser escalables y aplicables a futuras versiones de la aplicación.
4. **Mantenibilidad:** El código y las configuraciones deben ser fáciles de mantener y actualizar para asegurar la continuidad de las medidas de seguridad.

## Implementación en Python

### Explicación Paso a Paso del Código

Para realizar una auditoría de seguridad y pruebas de penetración en una aplicación Django, utilizaremos varias herramientas y técnicas. A continuación, se presenta una guía paso a paso para implementar estas prácticas.

#### Paso 1: Configuración del Entorno de Auditoría

Primero, necesitamos configurar un entorno de auditoría que nos permita revisar el código fuente y las configuraciones de la aplicación. Para ello, clonaremos el repositorio de la aplicación y configuraremos un entorno virtual.

```bash
# Clonar el repositorio
git clone https://github.com/tu_usuario/sistema_carrito_compras.git

# Crear y activar un entorno virtual
cd sistema_carrito_compras
python3 -m venv venv
source venv/bin/activate

# Instalar las dependencias
pip install -r requirements.txt
```

#### Paso 2: Revisión de Código Fuente

Utilizaremos herramientas como Bandit y pylint para revisar el código fuente en busca de vulnerabilidades y malas prácticas.

```bash
# Instalar Bandit y pylint
pip install bandit pylint

# Ejecutar Bandit para análisis de seguridad
bandit -r .

# Ejecutar pylint para análisis de calidad de código
pylint sistema_carrito_compras/
```

#### Paso 3: Evaluación de Configuraciones

Revisaremos las configuraciones de Django para asegurar que se siguen las mejores prácticas de seguridad.

```python
# settings.py

# Asegurarse de que DEBUG está desactivado en producción
DEBUG = False

# Configurar una lista de hosts permitidos
ALLOWED_HOSTS = ['tu_dominio.com']

# Configurar la seguridad de las cookies
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True

# Configurar HTTPS
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 3600
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
```

#### Paso 4: Análisis de Dependencias

Utilizaremos Safety para analizar las dependencias y detectar vulnerabilidades conocidas.

```bash
# Instalar Safety
pip install safety

# Ejecutar Safety para análisis de dependencias
safety check
```

#### Paso 5: Pruebas de Penetración

Para las pruebas de penetración, utilizaremos herramientas como OWASP ZAP y sqlmap.

##### Pruebas de Autenticación

Utilizaremos OWASP ZAP para realizar pruebas de autenticación y detectar vulnerabilidades como fuerza bruta y enumeración de usuarios.

```bash
# Ejecutar OWASP ZAP
zap.sh
```

##### Pruebas de Inyección de Código

Utilizaremos sqlmap para detectar vulnerabilidades de inyección SQL.

```bash
# Ejecutar sqlmap
sqlmap -u "http://tu_dominio.com/producto?id=1" --batch
```

### Código Fuente Completo y Comentado

A continuación, se presenta un ejemplo de cómo integrar algunas de estas prácticas en el código de la aplicación.

```python
# settings.py

# Configuraciones de seguridad
DEBUG = False
ALLOWED_HOSTS = ['tu_dominio.com']

# Seguridad de las cookies
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True

# Configuración de HTTPS
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 3600
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True

# Middleware de seguridad
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# Auditoría de seguridad con Bandit y pylint
# Ejecutar los siguientes comandos desde la terminal
# bandit -r .
# pylint sistema_carrito_compras/

# Análisis de dependencias con Safety
# Ejecutar el siguiente comando desde la terminal
# safety check
```

### Ejemplos de Uso y Pruebas Unitarias

Para asegurar que las configuraciones de seguridad funcionan correctamente, podemos implementar pruebas unitarias.

```python
# tests.py

from django.test import TestCase
from django.urls import reverse

class SecurityTests(TestCase):
    def test_https_redirect(self):
        response = self.client.get(reverse('home'), secure=False)
        self.assertEqual(response.status_code, 301)
        self.assertTrue(response.url.startswith('https://'))

    def test_csrf_cookie_secure(self):
        response = self.client.get(reverse('home'))
        self.assertTrue(response.cookies['csrftoken'].secure)

    def test_session_cookie_secure(self):
        response = self.client.get(reverse('home'))
        self.assertTrue(response.cookies['sessionid'].secure)
```

### Mejores Prácticas y Consideraciones de Diseño

#### Seguridad

1. **Revisión Continua:** Realiza auditorías de seguridad y pruebas de penetración de manera regular, especialmente después de realizar cambios significativos en la aplicación.
2. **Actualización de Dependencias:** Mantén las dependencias actualizadas para evitar vulnerabilidades conocidas.
3. **Principio de Mínimos Privilegios:** Configura los permisos y accesos siguiendo el principio de mínimos privilegios.

#### Rendimiento

1. **Entorno de Pruebas:** Realiza auditorías y pruebas de penetración en un entorno de pruebas que replique el entorno de producción para evitar afectar el rendimiento de la aplicación en producción.
2. **Optimización de Código:** Asegúrate de que las medidas de seguridad no introduzcan cuellos de botella en el rendimiento de la aplicación.

#### Escalabilidad

1. **Automatización:** Automatiza las auditorías de seguridad y pruebas de penetración utilizando herramientas de integración continua (CI) para asegurar que se realicen de manera consistente.
2. **Documentación:** Documenta las configuraciones y prácticas de seguridad para facilitar la escalabilidad y mantenimiento de la aplicación.

#### Depuración y Solución de Problemas Comunes

1. **Logs de Seguridad:** Configura logs detallados para registrar eventos de seguridad y facilitar la depuración de problemas.
2. **Monitoreo:** Implementa herramientas de monitoreo para detectar y responder rápidamente a incidentes de seguridad.

En conclusión, la auditoría de seguridad y las pruebas de penetración son prácticas esenciales para asegurar que tu sistema de carrito de compras con Django sea seguro y resistente a ataques. Siguiendo las mejores prácticas y utilizando las herramientas adecuadas, puedes identificar y mitigar vulnerabilidades, asegurando la integridad y confidencialidad de los datos de tus usuarios.

