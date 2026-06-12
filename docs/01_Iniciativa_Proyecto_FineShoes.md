# Documento de Iniciativa del Proyecto de Software - Fine Shoes

**Nombre dueño del producto:** [Nombre del integrante 1]
**Responsable / Supervisor (Scrum Master):** [Nombre del integrante 2]
**Área:** Ventas y gestión de inventario de calzado (e-commerce)
**Número de Solicitud o Petición:** 01

## 1. Resumen Ejecutivo

Fine Shoes es una tienda en línea de sneakers (tenis) que permite a los clientes
navegar un catálogo de productos, registrarse, agregar artículos a un carrito de
compras y generar pedidos. El sistema cuenta además con un panel administrativo
para la gestión de productos, inventario, pedidos y reportes de ventas. El
desarrollo se realizará bajo la metodología ágil Scrum.

## 2. Objetivo General

Desarrollar un sistema integral de comercio electrónico para la venta de calzado,
que permita la gestión de usuarios, catálogo de productos, inventario, carrito de
compras, pedidos y reportes administrativos.

## 3. Objetivos Específicos

- Permitir el registro y autenticación segura de usuarios (clientes y administradores).
- Ofrecer un catálogo de productos filtrable por marca, talla, color y precio.
- Gestionar el inventario por variante de producto (talla/color).
- Implementar un carrito de compras y un flujo de checkout (generación de pedidos).
- Generar reportes de ventas, productos más vendidos y alertas de stock bajo.
- Integrar una API externa (DummyJSON) para importar productos de ejemplo.

## 4. Alcance

Incluye los módulos de:

- Gestión de usuarios y autenticación (JWT, roles cliente/admin).
- Catálogo de productos e inventario.
- Carrito de compras.
- Pedidos (checkout, historial, cambio de estatus).
- Reportes administrativos (ventas, productos top, stock bajo).
- Importación de productos desde una API externa pública.

## 5. Fuera de Alcance

- Integración con pasarelas de pago reales (Stripe, PayPal, etc.).
- Aplicación móvil nativa.
- Notificaciones push o por correo electrónico.
- Integración con sistemas de envíos/paquetería.

## 6. Requerimientos (Resumen)

Historias de usuario principales definidas en el backlog:

1. Registro, autenticación y gestión de usuarios (roles cliente/admin).
2. Catálogo de productos e inventario por variante (talla/color).
3. Carrito de compras.
4. Checkout y gestión de pedidos.
5. Reportes administrativos y alertas de stock bajo.
6. Importación de productos desde API externa (DummyJSON).

## 7. Stack Tecnológico

| Capa          | Tecnología                       |
| ------------- | --------------------------------- |
| Frontend      | HTML5 + CSS3 + JavaScript (ES Modules) |
| Backend / API | Python 3.11 + FastAPI            |
| Base de datos | MySQL 8.0                        |
| Autenticación | JWT (python-jose + bcrypt)       |
| API externa   | DummyJSON (productos de ejemplo) |
| Gráficas      | Chart.js 4                       |

## 8. Equipo del Proyecto

| Nombre | Rol |
| ------ | --- |
| [Integrante 1] | Líder de Proyecto / Scrum Master |
| [Integrante 2] | Analista de Requerimientos |
| [Integrante 3] | Desarrollador Backend (Dev A) |
| [Integrante 4] | Desarrollador Frontend (Dev B) |
| [Integrante 5] | Tester / QA |

## 9. Riesgos

- Retrasos en la entrega de incrementos por carga académica.
- Cambios de alcance solicitados por el "cliente" (profesor).
- Dependencias técnicas entre módulos (ej. inventario y pedidos).
- Conflictos de código por trabajo simultáneo de varios desarrolladores.

## 10. KPIs

- Velocidad del equipo (Story Points completados por sprint).
- Cumplimiento de sprints (entregas a tiempo).
- Número de bugs detectados por sprint.

## 11. Entregables

- Incrementos funcionales del sistema (frontend + backend + base de datos).
- Documentación técnica (este documento, backlog, minutas, manual de instalación).
- Repositorio en GitHub con el código fuente versionado.
