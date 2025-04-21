# 📌 Informe de Modelado de Datos y Consultas Avanzadas en Oracle

Este repositorio contiene el análisis, diseño y consultas avanzadas realizadas sobre una base de datos relacional utilizando Oracle SQL Developer y SQL Developer Data Modeler.

---

## 🔄 Consultas Avanzadas (Bloque 2)

### 🧾 Superconsulta con WITH, RANK, LISTAGG

**Objetivo:** Mostrar por cada cliente que realizó pedidos en 1997:
- Número de productos distintos comprados
- Lista de productos comprados (por nombre)
- Producto más comprado (por cantidad)

```sql
WITH pedidos_1997 AS (
  SELECT * FROM orders
  WHERE EXTRACT(YEAR FROM order_date) = 1997
),
detalle_1997 AS (
  SELECT o.customer_id, p.name AS producto, od.quantity
  FROM pedidos_1997 o
  JOIN order_details od ON o.id = od.order_id
  JOIN products p ON od.product_id = p.id
),
ranking_productos AS (
  SELECT customer_id, producto, quantity,
         RANK() OVER (PARTITION BY customer_id ORDER BY quantity DESC) AS ranking
  FROM detalle_1997
)
SELECT customer_id,
       COUNT(DISTINCT producto) AS productos_distintos,
       LISTAGG(producto, ', ') WITHIN GROUP (ORDER BY producto) AS lista_productos,
       MAX(CASE WHEN ranking = 1 THEN producto END) AS producto_top
FROM ranking_productos
GROUP BY customer_id;
```

### 🔍 Plan de Ejecución (`EXPLAIN PLAN`)

| ID | Operación                 | Descripción |
|----|---------------------------|-------------|
| 9  | `TABLE ACCESS FULL`      | Lee toda la tabla `ORDER_DETAILS` |
| 8  | `SORT JOIN`              | Ordena para merge con `ORDERS` |
| 6  | `TABLE ACCESS BY INDEX ROWID` | Lee `ORDERS` con índice |
| 5  | `MERGE JOIN`             | Une `ORDERS` con `ORDER_DETAILS` |
|10  | `TABLE ACCESS FULL`      | Lee `PRODUCTS` completamente |
| 4  | `HASH JOIN`              | Une con `PRODUCTS` |
| 3  | `WINDOW SORT`            | Aplica `RANK()` por cliente |
| 2  | `VIEW`                   | Representa la CTE intermedia |
| 1  | `SORT GROUP BY`          | Agrupa resultados finales |

**Coste total**: 10 — muy eficiente.

---

Este ejemplo muestra un uso profesional de SQL para generación de reportes avanzados, ideal para entornos de banca y análisis de clientes.
