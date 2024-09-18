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