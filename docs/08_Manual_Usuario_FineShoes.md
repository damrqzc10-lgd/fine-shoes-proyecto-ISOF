# Manual de Usuario - Fine Shoes

**Sistema de Comercio Electrónico de Calzado**
Versión del documento: 1.0
Fecha: 12/06/2026

---

## 1. Introducción

Fine Shoes es una tienda en línea de calzado (sneakers) que permite a los
clientes navegar un catálogo de productos, registrarse, gestionar un carrito
de compras y realizar pedidos. Cuenta además con un panel administrativo para
la gestión de productos, inventario, pedidos y reportes de ventas.

Este manual describe el uso de cada módulo del sistema, tanto para el rol
**Cliente** como para el rol **Administrador**.

## 2. Requisitos para Ejecutar el Sistema

- Tener instalado Python 3.11 o superior.
- Tener instalado y en ejecución un servidor MySQL 8.0.
- Tener un navegador web actualizado (Chrome, Firefox o Edge).
- Conexión a internet (para la importación de productos desde la API externa DummyJSON).

## 3. Instalación y Puesta en Marcha

1. Clonar el repositorio del proyecto desde GitHub:
   ```
   git clone https://github.com/<usuario>/fine-shoes-proyecto-ISOF.git
   cd fine-shoes-proyecto-ISOF
   ```
2. Crear y activar un entorno virtual de Python:
   ```
   python -m venv venv
   venv\Scripts\activate   (en Windows)
   source venv/bin/activate   (en Linux/Mac)
   ```
3. Instalar las dependencias del backend:
   ```
   pip install -r requirements.txt
   ```
4. Configurar la conexión a la base de datos MySQL (usuario, contraseña, nombre
   de la base de datos) en el archivo de configuración del backend.
5. Ejecutar las migraciones / scripts de creación de tablas en la base de
   datos `fine_shoes`.
6. Iniciar el servidor backend (FastAPI):
   ```
   uvicorn main:app --reload
   ```
7. Abrir el frontend (archivos HTML) en el navegador, o servirlos con un
   servidor local simple.

## 4. Acceso al Sistema

### 4.1 Registro de Cuenta (Cliente)

1. En la pantalla principal, hacer clic en **"Registrarse"**.
2. Llenar el formulario con: nombre completo, correo electrónico y contraseña.
3. Hacer clic en **"Crear cuenta"**.
4. El sistema valida que el correo tenga formato correcto y que no exista
   previamente otra cuenta con el mismo correo.
5. Tras un registro exitoso, el sistema redirige a la pantalla de inicio de
   sesión.

### 4.2 Inicio de Sesión

1. Hacer clic en **"Iniciar Sesión"**.
2. Ingresar el correo electrónico y la contraseña registrados.
3. Hacer clic en **"Entrar"**.
4. Si las credenciales son correctas, el sistema genera una sesión (token
   JWT) y muestra el nombre del usuario en la barra de navegación.
5. Si las credenciales son incorrectas, se muestra un mensaje de error y no
   se concede acceso.

### 4.3 Perfil de Usuario

1. Hacer clic en el nombre de usuario (esquina superior derecha) → **"Mi
   perfil"**.
2. Se muestran los datos registrados (nombre, correo).
3. Es posible editar el nombre y, opcionalmente, cambiar la contraseña.
4. Hacer clic en **"Guardar cambios"** para confirmar.

## 5. Módulo de Catálogo (Cliente)

### 5.1 Explorar el Catálogo

1. Desde la página principal, se muestra el listado de productos (tenis)
   disponibles, con imagen, nombre, marca y precio.
2. Es posible navegar entre páginas si el catálogo tiene muchos productos.

### 5.2 Filtrar Productos

1. En la parte superior/lateral del catálogo se encuentran los filtros:
   **Marca**, **Talla**, **Color** y **Rango de precio**.
2. Seleccionar uno o varios filtros; la lista de productos se actualiza
   automáticamente.
3. Es posible también escribir un texto en el buscador para encontrar
   productos por nombre.
4. Si un producto no cuenta con stock en ninguna variante, se muestra
   etiquetado como **"Agotado"**.

### 5.3 Ver Detalle de un Producto

1. Hacer clic sobre cualquier producto del catálogo.
2. Se muestra: imagen, descripción, marca, precio y las variantes disponibles
   (talla y color) con su stock correspondiente.
3. Seleccionar la talla/color deseado antes de agregar al carrito.

## 6. Módulo de Carrito de Compras (Cliente)

### 6.1 Agregar un Producto al Carrito

1. Desde el detalle del producto, seleccionar talla y color.
2. Indicar la cantidad deseada (no puede exceder el stock disponible).
3. Hacer clic en **"Agregar al carrito"**.
4. El producto aparece en el ícono del carrito, ubicado en la barra de
   navegación.

### 6.2 Ver y Modificar el Carrito

1. Hacer clic en el ícono del carrito.
2. Se muestra la lista de productos agregados, con cantidad, precio unitario
   y subtotal por línea.
3. Es posible aumentar/disminuir la cantidad de cada producto; el total se
   recalcula automáticamente.
4. Para eliminar un producto, hacer clic en el ícono de basura junto a la
   línea correspondiente.
5. El botón **"Vaciar carrito"** elimina todos los productos del carrito.

## 7. Módulo de Pedidos (Cliente)

### 7.1 Generar un Pedido (Checkout)

1. Desde el carrito, hacer clic en **"Proceder al pago"** o **"Confirmar
   pedido"**.
2. Revisar el resumen del pedido (productos, cantidades, total).
3. Hacer clic en **"Confirmar"**.
4. El sistema valida que exista stock suficiente para cada producto. Si todo
   es correcto, se genera el pedido y se descuenta el stock correspondiente;
   el carrito queda vacío.
5. Si algún producto ya no tiene stock suficiente, el sistema cancela toda la
   operación y muestra un mensaje de error, sin modificar el stock de los
   demás productos.

### 7.2 Consultar Mis Pedidos

1. Ir al menú **"Mis Pedidos"**.
2. Se muestra el historial de pedidos realizados, con fecha, total y estatus
   actual.
3. Los posibles estatus son: **Pendiente**, **Procesando**, **Enviado**,
   **Entregado**.
4. Al seleccionar un pedido, se muestra el detalle de los productos incluidos
   y la línea de tiempo de su estatus.

## 8. Panel Administrativo (Administrador)

> El acceso al panel administrativo requiere iniciar sesión con una cuenta
> que tenga el rol `admin`.

### 8.1 Gestión de Productos e Inventario

1. Ir al menú **"Administración" → "Productos"**.
2. Se muestra la lista de productos con sus variantes (talla, color, stock).
3. **Agregar producto:** clic en "Nuevo producto", llenar nombre, marca,
   descripción, precio e imagen; el sistema genera el SKU automáticamente.
4. **Editar producto:** clic en el ícono de edición junto al producto deseado,
   modificar los datos y guardar.
5. **Eliminar producto:** clic en el ícono de basura; se solicitará
   confirmación.
6. **Actualizar stock:** dentro del producto, editar la cantidad de cada
   variante (talla/color). El sistema no permite valores negativos.
7. Las filas de variantes se muestran en **rojo** si el stock es 0, y en
   **amarillo** si está por debajo del mínimo configurado, para identificar
   rápidamente productos en "Stock Crítico".

### 8.2 Importar Productos desde API Externa

1. Ir al menú **"Administración" → "Productos" → "Importar desde API"**.
2. Hacer clic en **"Importar productos"**.
3. El sistema consulta la API pública DummyJSON y agrega los productos nuevos
   al catálogo (sin generar duplicados si ya existen).

### 8.3 Gestión de Pedidos

1. Ir al menú **"Administración" → "Pedidos"**.
2. Se muestra la lista de todos los pedidos realizados por los clientes, con
   su estatus actual.
3. Para actualizar el estatus de un pedido, seleccionar el nuevo estatus
   (Pendiente → Procesando → Enviado → Entregado) y guardar.
4. Cada cambio de estatus queda registrado con fecha y hora en la bitácora del
   pedido.

### 8.4 Reportes y Dashboard

1. Ir al menú **"Administración" → "Reportes"** o **"Dashboard"**.
2. Seleccionar el rango de fechas a consultar.
3. El sistema muestra:
   - Resumen de ventas del periodo.
   - Gráfica de ventas por mes.
   - Productos más vendidos.
   - Alerta de productos con stock bajo.
4. Si la "Fecha Inicio" es posterior a la "Fecha Fin", el sistema muestra un
   mensaje de error y no genera el reporte.

## 9. Cierre de Sesión

1. Hacer clic en el nombre de usuario (esquina superior derecha).
2. Seleccionar **"Cerrar sesión"**.
3. El sistema elimina la sesión activa y redirige a la pantalla de inicio de
   sesión.

## 10. Preguntas Frecuentes

**¿Qué hago si olvidé mi contraseña?**
Actualmente el sistema no cuenta con recuperación automática de contraseña;
contactar al administrador para restablecerla manualmente.

**¿Por qué no puedo agregar más unidades de un producto al carrito?**
El sistema valida que la cantidad solicitada no exceda el stock disponible de
esa variante (talla/color).

**¿Qué pasa si dos clientes compran el último producto disponible al mismo
tiempo?**
El sistema procesa los pedidos de forma transaccional: solo uno de los
pedidos se completará exitosamente y el otro será rechazado con un mensaje de
error, sin afectar el stock de otros productos.

---

**Repositorio del proyecto:** https://github.com/damrqzc10-lgd/fine-shoes-proyecto-ISOF
