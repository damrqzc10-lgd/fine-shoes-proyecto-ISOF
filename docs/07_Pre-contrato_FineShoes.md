# Contrato - Fine Shoes

## 1. Información del Proyecto

### 1.1 Información General

| Campo | Valor |
| --- | --- |
| Petición | 01 |
| Sistema | Fine Shoes (Tienda en línea de calzado) |
| Módulo | Ventas, Inventario, Usuarios y Reportes |
| Opción | Catálogo, Carrito, Pedidos, Administración |
| Nombre | Sistema de comercio electrónico Fine Shoes |
| Área | Ventas y operaciones de la mueblería/tienda (e-commerce) |

### 1.2 Alcance

Las áreas y opciones impactadas son las siguientes:

- Función Usuarios
  - Registro
  - Autenticación (login/JWT)
  - Perfil de usuario
- Función Catálogo
  - Alta, edición y baja de productos
  - Gestión de inventario por variante (talla/color)
  - Importación de productos desde API externa (DummyJSON)
- Función Carrito
  - Agregar, modificar y eliminar productos del carrito
- Función Pedidos
  - Checkout (creación de pedido)
  - Historial de pedidos del cliente
  - Gestión de estatus de pedidos (administrador)
- Función Reportes
  - Resumen de ventas
  - Top de productos vendidos
  - Ventas por mes
  - Alertas de stock bajo

### 1.3 Supuestos

- El sistema operará únicamente en ambiente de desarrollo/pruebas (entorno local).
- No se procesan pagos reales; el checkout simula la generación del pedido.
- Los datos de productos pueden complementarse con la API externa DummyJSON.

## 2. Descripción Detallada de Requerimientos

### Registro de usuario
**Estatus:** Propuesto
El sistema Fine Shoes debe permitir que un visitante cree una cuenta indicando
nombre, correo electrónico y contraseña, almacenando la contraseña cifrada
(bcrypt) en la tabla `usuarios` de la base de datos `fine_shoes` (MySQL).

### Validar usuario
**Estatus:** Propuesto
El sistema debe validar las credenciales del usuario contra la tabla `usuarios`
al iniciar sesión, y debe contar con un rol asignado (`customer` o `admin`) que
determine los permisos dentro del sistema.

### Grabar producto / inventario
**Estatus:** Propuesto
El sistema debe permitir al administrador grabar un producto en la tabla
`productos` y sus variantes (talla/color/stock) en la tabla `inventario`,
generando un SKU único.

### Consultar catálogo
**Estatus:** Propuesto
El sistema debe permitir a cualquier visitante consultar el catálogo de
productos activos, con filtros por marca, talla, color, precio y texto de
búsqueda.

### Gestionar carrito
**Estatus:** Propuesto
El sistema debe permitir a un usuario autenticado agregar, modificar cantidad y
eliminar productos de su carrito, validando que la cantidad no exceda el stock
disponible.

### Generar pedido (checkout)
**Estatus:** Propuesto
El sistema debe permitir convertir el contenido del carrito en un pedido,
descontando el stock correspondiente de forma transaccional (todo o nada).

### Consultar y actualizar estatus de pedidos
**Estatus:** Propuesto
El sistema debe permitir al cliente consultar el historial y estatus de sus
pedidos, y al administrador actualizar dicho estatus (`Pendiente`,
`Procesando`, `Enviado`, `Entregado`).

### Generar reportes
**Estatus:** Propuesto
El sistema debe generar reportes de ventas (resumen, por producto, por mes) y
un reporte de productos con stock bajo, visibles únicamente para el rol
`admin`.

---

**Versión del formato:** 1.0
