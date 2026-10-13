@echo off
chcp 65001 > nul
title Actividad 9: Extracción Avanzada y Análisis Relacional en Omnibank

echo ==============================================================================
echo   ACTIVIDAD 9: EXTRACCIÓN AVANZADA Y ANÁLISIS RELACIONAL EN OMNIBANK
echo ==============================================================================
echo.

echo [1. CONTEO GLOBAL DE REGISTROS]
echo SELECT
echo   (SELECT COUNT(*) FROM core.customers) AS customers_count,
echo   (SELECT COUNT(*) FROM core.accounts) AS accounts_count,
echo   (SELECT COUNT(*) FROM core.transactions) AS transactions_count;
echo.

echo [2. REPORTE GENERAL DE MOVIMIENTOS]
echo SELECT
echo   t.transaction_id,
echo   c.first_name,
echo   c.last_name,
echo   a.account_number,
echo   t.tx_type,
echo   t.amount
echo FROM core.transactions AS t
echo INNER JOIN core.accounts AS a ON t.account_id = a.account_id
echo INNER JOIN core.customers AS c ON a.customer_id = c.customer_id
echo ORDER BY t.tx_timestamp, t.transaction_id;
echo.

echo [3. REPORTE DE CUENTAS SIN MOVIMIENTOS]
echo SELECT
echo   a.account_number,
echo   a.is_active,
echo   CASE
echo     WHEN a.is_active = TRUE THEN 'ACTIVA - SIN MOVIMIENTOS'
echo     WHEN a.is_active = FALSE THEN 'DESACTIVADA - SIN MOVIMIENTOS'
echo     ELSE 'ESTADO NO INFORMADO - SIN MOVIMIENTOS'
echo   END AS account_status_label
echo FROM core.accounts AS a
echo LEFT JOIN core.transactions AS t ON a.account_id = t.account_id
echo WHERE t.transaction_id IS NULL
echo ORDER BY a.account_number;
echo.

echo [4. REPORTE DE TITULARES CON CUENTA ELEGIBLE]
echo SELECT
echo   c.email,
echo   a.account_number,
echo   a.balance
echo FROM core.customers AS c
echo LEFT JOIN core.accounts AS a ON c.customer_id = a.customer_id AND a.balance > 0
echo ORDER BY c.customer_id, a.account_number;
echo.

echo [5. NOTAS DE CONTROL Y ANÁLISIS]
echo - Grano: Una fila por movimiento (transaction) / una fila por cuenta sin movimientos / una fila por cuenta con balance mayor a 0 asociada a su titular.
echo - Ordenamiento y Estabilización: Se usa ORDER BY con timestamp y transaction_id para eventos concurrentes.
echo - Interpretación de cuentas sin movimientos: Las cuentas inactivas (is_active = FALSE) vs activas (is_active = TRUE) reflejan situaciones distintas en el historial operativo.
echo.

pause

Lo que aprendi:
Aprendi a usar extraccion de datos con INNER JOIN y LEFT JOIN para identificar estados de cuentas operativas, inactivas o sin transacciones registradas

FIX
# Day 09 Answer Key — Relaciones Reales y Ausencia de Movimientos

## Expected Result

El checkpoint aporta cuatro clientes, cinco cuentas y cuatro movimientos. El recorrido de eventos devuelve cuatro filas; la consulta de ausencia devuelve ACCT-1002 desactivada y ACCT-3002 activa. El reporte de titulares con balance > 0 conserva a los cuatro clientes, con columnas de cuenta nulas para Alice y Humberto. La variante con el filtro en `WHERE` pierde esas dos personas.

## Reference Solution

```sql
SELECT (SELECT COUNT(*) FROM core.customers) AS customers,
       (SELECT COUNT(*) FROM core.accounts) AS accounts,
       (SELECT COUNT(*) FROM core.transactions) AS movements;

SELECT t.transaction_id, c.first_name, c.last_name, a.account_number, t.tx_type, t.amount
FROM core.transactions t
JOIN core.accounts a ON a.account_id = t.account_id
JOIN core.customers c ON c.customer_id = a.customer_id
ORDER BY t.tx_timestamp, t.transaction_id;

SELECT a.account_number, a.is_active,
       CASE WHEN a.is_active THEN 'ACTIVA SIN MOVIMIENTOS'
            ELSE 'DESACTIVADA SIN MOVIMIENTOS' END AS estado
FROM core.accounts a
LEFT JOIN core.transactions t ON t.account_id = a.account_id
WHERE t.transaction_id IS NULL
ORDER BY a.account_number;

SELECT c.email, a.account_number, a.balance
FROM core.customers c
LEFT JOIN core.accounts a
  ON a.customer_id = c.customer_id AND a.balance > 0
ORDER BY c.email, a.account_number;

-- Comparación diagnóstica: WHERE elimina las filas externas sin pareja elegible.
SELECT c.email, a.account_number, a.balance
FROM core.customers c
LEFT JOIN core.accounts a ON a.customer_id = c.customer_id
WHERE a.balance > 0
ORDER BY c.email, a.account_number;
```

La última consulta devuelve solo John y Jane; si se requiere preservar a los titulares, el predicado de cuenta elegible pertenece a `ON`. `is_active` y ausencia de movimiento son atributos diferentes y no deben codificarse en el enum `tx_type`.

## Common Valid Variations

* Se aceptan alias distintos, una etiqueta textual equivalente y un `RIGHT JOIN` reorientado que preserve las mismas cuentas. 
* Una solución con `NOT EXISTS` para la ausencia también es válida si está claramente explicada, aunque no se exige antes del día 12.

## Common Mistakes

* **Unir movimiento con cliente por un campo descriptivo**: Hacerlo en vez de recorrer las claves foráneas crea asociaciones falsas.
* **Filtrar `transactions` en `WHERE` después de un `LEFT JOIN`**: Puede ocultar cuentas sin eventos inadvertidamente.
* **Usar un `INNER JOIN` entre clientes y cuentas**: Elimina a Humberto del reporte.
* **Filtrar `balance` en `WHERE`**: Elimina también a Alice del reporte.

## Grading Rubric

* **25%**: Recuentos de control y uso de claves reales (`account_id` y `customer_id`).
* **25%**: Recorrido de cuatro movimientos, una fila por evento y orden estable.
* **25%**: Ausencia de dos cuentas con `is_active` separado de la existencia de eventos.
* **25%**: Cuatro titulares preservados, comparación entre predicados en `ON` vs `WHERE` y explicación del grano.
