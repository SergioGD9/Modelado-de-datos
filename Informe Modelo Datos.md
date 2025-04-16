# 📑 Informe Técnico - Modelo de Datos W3S

Este documento analiza el modelo relacional del esquema `W3S`, importado y visualizado mediante Oracle SQL Developer Data Modeler.

---

## 🔄 Propósito

El modelo representa un sistema de gestión de pedidos de productos, con relaciones entre:
- Clientes
- Pedidos
- Productos
- Empleados
- Proveedores
- Transportistas

---

## 📊 Diagrama Entidad-Relación (ERD)

![Modelo ERD](./img/Display_1.png)

Este diagrama visualiza las entidades principales y sus relaciones. Las líneas entre tablas indican claves foráneas.

---

## 📂 Relaciones principales

| Tabla Origen  | Columna       | Tabla Destino | Relación |
|---------------|---------------|----------------|-----------|
| ORDERS        | CUSTOMER_ID   | CUSTOMERS.ID   | 1:N       |
| ORDERS        | EMPLOYEE_ID   | EMPLOYEES.ID   | 1:N       |
| ORDERS        | SHIPPER_ID    | SHIPPERS.ID    | 1:N       |
| ORDER_DETAILS | ORDER_ID      | ORDERS.ID      | 1:N       |
| ORDER_DETAILS | PRODUCT_ID    | PRODUCTS.ID    | 1:N       |
| PRODUCTS      | SUPPLIER_ID   | SUPPLIERS.ID   | 1:N       |
| PRODUCTS      | CATEGORY_ID   | CATEGORIES.ID  | 1:N       |

---

## 📃 Informe HTML

El archivo `report_data.html` contiene el resumen generado automáticamente por SQL Data Modeler. Incluye:

- Tablas y campos
- Tipos de datos
- Restricciones (PK, FK, CHECK)
- Índices

🔗 Se puede abrir en cualquier navegador web para navegar entre entidades.

---

## 🔢 Análisis técnico por tabla (ejemplo)

### `ORDERS`
- **ID** (PK)
- **CUSTOMER_ID** (FK) → CUSTOMERS.ID
- **EMPLOYEE_ID** (FK) → EMPLOYEES.ID
- **ORDER_DATE** (DATE)
- **SHIPPER_ID** (FK) → SHIPPERS.ID

### `ORDER_DETAILS`
- Compuesta por `ORDER_ID` y `PRODUCT_ID` como PK compuesta
- Contiene `QUANTITY`
- Se relaciona con `PRODUCTS` y `ORDERS`

---

## 📌 Observaciones

- El modelo está normalizado (3FN)
- Las relaciones se representan correctamente por claves foráneas
- Los índices fueron definidos en campos clave (ej. `CUSTOMER_ID`, `ORDER_DATE`)

---

Este informe sirve como soporte para cualquier auditoría técnica, mantenimiento de modelo o generación de scripts para despliegue.
