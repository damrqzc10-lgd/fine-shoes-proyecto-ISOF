# Backlog de Historias de Usuario - Fine Shoes

Equipo de 5 integrantes.

Se aplica el principio de "Divide y Vencerás"
(descomposición de épicas), separando Backend, Frontend y QA, para permitir
trabajo paralelo sin pisarse el código (conflictos de Git).

Roles:

- **Dev A** – Backend (FastAPI / Python / MySQL)
- **Dev B** – Frontend (HTML/CSS/JS - vistas cliente)
- **Dev C** – Frontend (HTML/CSS/JS - panel administrativo)
- **QA / Tester** – Pruebas y validación
- **Analista / Scrum Master** – Documentación, minutas, coordinación

---

## ÉPICA 1: Gestión de Usuarios y Autenticación

### HU-01A (Dev A - Backend): API de Autenticación y Modelado de Usuarios (8h)

- **Rol Destino:** Cliente / Administrador.
- **Descripción:**
  - **Como** visitante de Fine Shoes,
  - **Quiero** poder registrarme e iniciar sesión de forma segura,
  - **Para** acceder a mi cuenta, carrito y pedidos.
- **Especificaciones Técnicas (FastAPI + MySQL):**
  - Tabla `usuarios`: id, nombre, email (unique), password_hash, rol (ENUM: 'customer', 'admin'), estado (boolean).
  - Cifrado de contraseñas con `bcrypt`.
  - Endpoints: `POST /auth/register`, `POST /auth/login` (retorna JWT), `GET /auth/me`, `PUT /auth/me`.
- **Criterios de Aceptación:**
  - [ ] Si las credenciales son incorrectas, la API responde con código 401 Unauthorized.
  - [ ] Un login exitoso retorna un token JWT que contiene id, email y rol del usuario.
  - [ ] No se permite registrar dos usuarios con el mismo correo electrónico (400 Bad Request).

### HU-01B (Dev B - Frontend): Interfaz de Registro, Login y Perfil (8h)

- **Rol Destino:** Cliente.
- **Descripción:**
  - **Como** cliente de Fine Shoes,
  - **Quiero** una interfaz para registrarme, iniciar sesión y editar mi perfil,
  - **Para** gestionar mi cuenta y mis compras.
- **Especificaciones Técnicas:**
  - Páginas `login.html` y `register.html` consumiendo `js/api.js`.
  - Validación de formularios en cliente (formato de email, contraseña mínima).
  - Guardar el token JWT y mostrar el estado de sesión en la navegación.
- **Criterios de Aceptación:**
  - [ ] El botón "Registrarse" permanece deshabilitado hasta que el correo tenga formato válido y los campos obligatorios estén llenos.
  - [ ] La interfaz se adapta correctamente a pantallas móviles y de escritorio.

### HT-01-QA (Tester): Plan de Pruebas de Autenticación (4h)

- **Rol Destino:** QA Engineer.
- **Propósito:** Validar que el registro, login y control de roles funcionen sin brechas de seguridad.
- **Tareas de QA:**
  - [ ] Suite de pruebas para validar expiración y estructura del JWT.
  - [ ] Probar inyección básica en el formulario de login para confirmar la sanitización de inputs.
  - [ ] Validar que un usuario `customer` reciba error al intentar acceder a rutas de `/admin`.

---

## ÉPICA 2: Catálogo de Productos e Inventario

### HU-02A (Dev A - Backend): CRUD de Productos e Inventario por Variante (10h)

- **Rol Destino:** Administrador.
- **Descripción:**
  - **Como** administrador de Fine Shoes,
  - **Quiero** registrar productos (tenis) con sus variantes de talla y color,
  - **Para** mantener un catálogo centralizado con precios y existencias.
- **Especificaciones Técnicas:**
  - Tablas `productos` (id, sku, nombre, marca, descripción, precio, imagen_url, activo) e `inventario` (id, producto_id, talla, color, stock).
  - Endpoints: `GET /products/`, `GET /products/{id}`, `POST /products/`, `PUT /products/{id}`, `DELETE /products/{id}`, `PUT /products/{id}/inventory/{iid}`.
  - Generación de SKU único combinando marca + secuencial.
- **Criterios de Aceptación:**
  - [ ] Al guardar un producto, el sistema genera e inserta un SKU único.
  - [ ] El campo `precio` solo acepta valores numéricos positivos mayores a cero.

### HU-02B (Dev B - Frontend): Catálogo Público con Filtros (8h)

- **Rol Destino:** Cliente.
- **Descripción:**
  - **Como** cliente,
  - **Quiero** ver el catálogo de tenis y filtrarlo por marca, talla, color y precio,
  - **Para** encontrar fácilmente el producto que busco.
- **Especificaciones Técnicas:**
  - Página `index.html` consumiendo `GET /products/` con query params (`brand`, `size`, `color`, `q`, `price`).
  - Página `product.html` con detalle del producto e inventario disponible.
- **Criterios de Aceptación:**
  - [ ] Los filtros se pueden combinar (ej. marca + talla) sin recargar toda la página.
  - [ ] Si un producto no tiene stock en ninguna variante, se muestra como "Agotado".

### HU-02C (Dev C - Frontend Admin): Panel de Inventario y Alertas de Stock (8h)

- **Rol Destino:** Administrador.
- **Descripción:**
  - **Como** administrador,
  - **Quiero** ver y actualizar el stock de cada variante con alertas visuales,
  - **Para** identificar rápidamente qué productos están agotados o con stock bajo.
- **Especificaciones Técnicas:**
  - Página `admin/products.html` con tabla de variantes.
  - Lógica para pintar filas en rojo si `stock === 0` y en amarillo si `stock` está por debajo de un mínimo configurado.
- **Criterios de Aceptación:**
  - [ ] El campo de stock no permite valores negativos.
  - [ ] Existe un filtro para listar únicamente productos en "Stock Crítico".

### HT-02-QA (Tester): Verificación de Catálogo e Inventario (4h)

- **Propósito:** Asegurar la integridad de los datos de productos y el correcto comportamiento de las alertas.
- **Tareas de QA:**
  - [ ] Insertar datos de prueba (mínimo 20 productos) y validar paginación/rendimiento del catálogo.
  - [ ] Validar que el algoritmo de SKU no genere duplicados.
  - [ ] Forzar el stock de una variante a 0 vía API y verificar que la interfaz cambie de color inmediatamente.

---

## ÉPICA 3: Carrito de Compras

### HU-03A (Dev A - Backend): API de Carrito de Compras (8h)

- **Rol Destino:** Cliente.
- **Descripción:**
  - **Como** cliente autenticado,
  - **Quiero** agregar, modificar y eliminar productos de mi carrito,
  - **Para** preparar mi compra antes de hacer el pedido.
- **Especificaciones Técnicas:**
  - Tabla `carrito_items` (id, usuario_id, inventario_id, cantidad).
  - Endpoints: `GET /cart/`, `POST /cart/`, `PUT /cart/{id}`, `DELETE /cart/{id}`, `DELETE /cart/`.
- **Criterios de Aceptación:**
  - [ ] No se puede agregar al carrito una cantidad mayor al stock disponible (409 Conflict).
  - [ ] Al vaciar el carrito, todos los items del usuario se eliminan.

### HU-03B (Dev B - Frontend): Interfaz de Carrito (6h)

- **Rol Destino:** Cliente.
- **Descripción:**
  - **Como** cliente,
  - **Quiero** ver y modificar mi carrito antes de pagar,
  - **Para** confirmar mi pedido correctamente.
- **Especificaciones Técnicas:**
  - Página `cart.html` consumiendo los endpoints de carrito.
  - Cálculo de subtotal y total en tiempo real al cambiar cantidades.
- **Criterios de Aceptación:**
  - [ ] El total se recalcula automáticamente al cambiar la cantidad de un producto.
  - [ ] La interfaz no permite seleccionar una cantidad de 0 o negativa (debe usarse "eliminar" en su lugar).

### HT-03-QA (Tester): Validación del Carrito (3h)

- **Tareas de QA:**
  - [ ] Validar que el carrito respete el stock disponible al agregar varias unidades.
  - [ ] Verificar que el carrito persista correctamente entre sesiones del mismo usuario.

---

## ÉPICA 4: Pedidos (Checkout)

### HU-04A (Dev A - Backend): Motor de Checkout y Descuento de Stock (12h)

- **Rol Destino:** Cliente / Administrador.
- **Descripción:**
  - **Como** desarrollador,
  - **Quiero** una API transaccional para procesar pedidos desde el carrito,
  - **Para** garantizar que la venta descuente el stock de forma exacta y segura.
- **Especificaciones Técnicas:**
  - Tablas `pedidos` (cabecera) y `pedido_detalles` (líneas).
  - Endpoint `POST /orders/checkout`.
  - Transacciones MySQL (BEGIN/COMMIT/ROLLBACK) para evitar sobreventa.
- **Criterios de Aceptación:**
  - [ ] Antes de crear el pedido, el backend verifica que cada variante tenga stock suficiente.
  - [ ] Si algún artículo no tiene stock, toda la operación se cancela (ROLLBACK) devolviendo 409 Conflict, sin alterar el stock de los demás artículos.

### HU-04B (Dev B - Frontend): Checkout y "Mis Pedidos" (10h)

- **Rol Destino:** Cliente.
- **Descripción:**
  - **Como** cliente,
  - **Quiero** confirmar mi pedido y ver el historial y estatus de mis compras,
  - **Para** dar seguimiento a mis pedidos.
- **Especificaciones Técnicas:**
  - Página `checkout.html` (confirmación de pedido) y `orders.html` (`GET /orders/my`).
  - Mostrar línea de tiempo de estados: `Pendiente` → `Procesando` → `Enviado` → `Entregado`.
- **Criterios de Aceptación:**
  - [ ] Tras un checkout exitoso, el carrito del usuario queda vacío.
  - [ ] Cada pedido muestra su estatus actual y fecha de creación.

### HU-04C (Dev C - Frontend Admin): Gestión de Pedidos (Admin) (8h)

- **Rol Destino:** Administrador.
- **Descripción:**
  - **Como** administrador,
  - **Quiero** ver todos los pedidos y actualizar su estatus,
  - **Para** gestionar el cumplimiento de las órdenes.
- **Especificaciones Técnicas:**
  - Página `admin/orders.html` (`GET /orders/`, `PUT /orders/{id}/status`).
- **Criterios de Aceptación:**
  - [ ] Cada cambio de estatus queda registrado con fecha y hora.
  - [ ] Solo usuarios con rol `admin` pueden modificar el estatus (verificado vía `Depends(require_admin)`).

### HT-04-QA (Tester): Pruebas de Concurrencia y Totales (6h)

- **Propósito:** Asegurar que no haya sobreventas y que los totales/IVA sean correctos.
- **Tareas de QA:**
  - [ ] Simular dos checkouts simultáneos sobre una variante con stock = 1; uno debe completarse y el otro ser rechazado con error controlado.
  - [ ] Validar el cálculo de totales e impuestos comparando la respuesta de la API contra una hoja de cálculo de control.

---

## ÉPICA 5: Reportes e Integración Externa

### HU-05A (Dev A - Backend): Endpoints de Reportes (8h)

- **Rol Destino:** Administrador.
- **Descripción:**
  - **Como** administrador,
  - **Quiero** consultar estadísticas de ventas y stock,
  - **Para** tomar decisiones de reabastecimiento y promociones.
- **Especificaciones Técnicas:**
  - Endpoints: `GET /reports/summary`, `GET /reports/sales-by-product`, `GET /reports/sales-by-month`, `GET /reports/low-stock`.
  - Consultas SQL con agregaciones (`SUM`, `COUNT`, `GROUP BY`) y filtros por fecha.
- **Criterios de Aceptación:**
  - [ ] Si "fecha inicio" es posterior a "fecha fin", la API responde 400 Bad Request.
  - [ ] `low-stock` solo devuelve variantes por debajo del mínimo configurado.

### HU-05B (Dev C - Frontend Admin): Dashboard con Gráficas (8h)

- **Rol Destino:** Administrador.
- **Descripción:**
  - **Como** administrador,
  - **Quiero** un dashboard visual con gráficos de ventas,
  - **Para** identificar tendencias rápidamente.
- **Especificaciones Técnicas:**
  - Página `admin/dashboard.html` y `admin/reports.html` usando Chart.js 4.
  - Gráficas: ventas por mes, top productos vendidos, alertas de stock bajo.
- **Criterios de Aceptación:**
  - [ ] Las gráficas se actualizan al cambiar el rango de fechas seleccionado.

### HU-05C (Dev B - Backend/Frontend): Importación de Productos desde API Externa (6h)

- **Rol Destino:** Administrador.
- **Descripción:**
  - **Como** administrador,
  - **Quiero** importar productos de ejemplo desde una API pública,
  - **Para** poblar rápidamente el catálogo durante el desarrollo y pruebas.
- **Especificaciones Técnicas:**
  - Endpoint `POST /products/import-api` que consume `https://dummyjson.com/products/category/mens-shoes`.
  - Mapear nombre, descripción, precio e imagen al modelo de `productos`.
- **Criterios de Aceptación:**
  - [ ] La importación no crea productos duplicados si se ejecuta más de una vez (validación por nombre o SKU).

### HT-05-QA (Tester): Auditoría de Reportes (4h)

- **Tareas de QA:**
  - [ ] Comparar manualmente la sumatoria de ventas de un mes del dashboard contra la suma directa en la base de datos (pedidos con estatus `Entregado`).
  - [ ] Verificar que la importación desde DummyJSON no rompa el formato de precios ni imágenes.

---

## Estrategia de Asignación en el Tablero Kanban (WIP Limit)

Con 4-5 integrantes, se recomienda **WIP (Work In Progress) = número de desarrolladores**
(máximo una tarjeta "En Progreso" por persona).

Ejemplo de arranque del Sprint 1:

- **Dev A** toma: HU-01A (Auth API) → base del sistema.
- **Dev B** toma: HU-02B (Catálogo público) → puede avanzar con datos mock mientras Dev A termina el backend.
- **Dev C** toma: HU-02C (Panel de inventario) en paralelo.
- **QA** prepara el plan de pruebas de Autenticación (HT-01-QA) en paralelo.

Cuando alguien termina, pasa su tarjeta a "Pruebas/QA" para *Code Review* y toma
la siguiente tarjeta disponible del backlog.

Nota: durante la planificación en Trello, algunas historias de usuario fueron subdivididas en tareas más pequeñas para facilitar el trabajo paralelo del equipo.
