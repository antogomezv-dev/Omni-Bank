@echo off
chcp 65001 > nul
title Tarea 9: Análisis de Producto Cartesiano, Filtrado Estricto y Dictamen en Omnibank

echo ==============================================================================
echo   TAREA 9: ANÁLISIS DE PRODUCTO CARTESIANO, FILTRADO Y DICTAMEN (OMNIBANK)
echo ==============================================================================
echo.

echo [1. CONSULTA DE CONTEO Y PRODUCTO CARTESIANO]
echo SELECT
echo   (SELECT COUNT(*) FROM customers) AS total_customers,
echo   (SELECT COUNT(*) FROM accounts) AS total_accounts,
echo   (SELECT COUNT(*) FROM transactions) AS total_transactions,
echo   COUNT(*) AS total_cartesian_product
echo FROM transactions
echo CROSS JOIN accounts
echo CROSS JOIN customers;
echo.

echo [2. CONSULTA DE TRANSACCIONES FILTRADAS Y ORDENADAS]
echo SELECT
echo   cu.email,
echo   ac.account_number,
echo   tr.transactions_type,
echo   tr.amount,
echo   tr.created_at
echo FROM transactions tr
echo INNER JOIN accounts ac ON tr.account_id = ac.account_id
echo INNER JOIN customer cu ON ac.customer_id = cu.customer_id
echo WHERE cu.is_active = TRUE
echo   AND tr.amount >= 100
echo ORDER BY
echo   tr.created_at ASC,
echo   tr.transactions_id ASC;
echo.

echo [3. DICTAMEN TÉCNICO]
echo Las tablas contienen 4 clientes, 5 cuentas y 4 transacciones, generando 80 combinaciones
echo en el producto cartesiano (4 x 5 x 4). Estas combinaciones incluyen relaciones que no
echo corresponden entre clientes, cuentas y movimientos.
echo Al aplicar las condiciones de unión mediante las claves account_id y customer_id se
echo conservan únicamente las relaciones reales. El filtro adicional de clientes activos y
echo movimientos de $100 o más reduce aún más el resultado, por lo tanto, la diferencia se
echo debe primero a la corrección de las relaciones mediante JOIN y después a los filtros de WHERE.
echo.

pause

Lo que aprendi:
Aprendi la comprension y evaluacion del producto cartesiano (CROSS JOIN) su impacto operacional y correccion mediante uniones explicitas y condiciones WHERE

FIX
# Day 09 Answer Key — Producto cartesiano y JOIN

## Expected Result
Los recuentos de la semilla son 4, 5 y 4: el producto es 80. El reporte con claves muestra tres eventos de al menos 100, no 80 operaciones.

## Reference Solution
```sql
SELECT (SELECT COUNT(*) FROM core.customers) AS customers,
       (SELECT COUNT(*) FROM core.accounts) AS accounts,
       (SELECT COUNT(*) FROM core.transactions) AS transactions;

SELECT COUNT(*) AS combinations
FROM core.customers CROSS JOIN core.accounts CROSS JOIN core.transactions;

SELECT c.email, a.account_number, t.tx_type, t.amount, t.tx_timestamp
FROM core.transactions t
JOIN core.accounts a ON a.account_id = t.account_id
JOIN core.customers c ON c.customer_id = a.customer_id
WHERE c.is_active AND t.amount >= 100
ORDER BY t.tx_timestamp, t.transaction_id;
```

El `CROSS JOIN` enumera todas las combinaciones posibles de las tres relaciones. La unión con claves preserva la pertenencia de cada movimiento a una cuenta y de esta a un cliente. El número de combinaciones por sí solo no determina el plan físico, la memoria usada ni el tiempo de respuesta.

**Lectura de la solución:** Una consulta exploratoria sin condiciones de unión puede producir muchas combinaciones que parecen operaciones bancarias pero que no corresponden a ninguna relación real. El área de revisión quiere cuantificar ese error lógico antes de incorporar el reporte al repositorio. La tarea extiende los `JOIN` de la actividad y deja un archivo reutilizable para los días de agregaciones.

## Common Valid Variations
Se aceptan `USING (account_id)` en la primera unión y una CTE que prefiltre los movimientos, siempre que el recorrido conserve las claves reales y el orden final desempate por `transaction_id`.

## Common Mistakes
Omitir cualquiera de las dos condiciones de unión multiplica filas y atribuye movimientos a titulares ajenos. El filtro `amount >= 100` se aplica después de reconstruir la relación; ordenar solo por importe no cumple la secuencia temporal solicitada.

## Grading Rubric
- **35%:** Cuenta las filas de `customers`, `accounts` y `transactions` y calcula su producto. Usa `CROSS JOIN` únicamente para obtener `COUNT(*)`; registra la cifra observada sin extraer el producto completo.
- **30%:** Reescribe el reporte uniendo cada movimiento con su cuenta y después con su titular. Filtra clientes activos y movimientos desde 100, proyecta correo, cuenta, tipo, importe y fecha, y añade un orden estable.
- **20%:** Comprueba el resultado con datos reproducibles: Los recuentos de la semilla son 4, 5 y 4: el producto es 80. El reporte con claves muestra tres eventos de al menos 100, no 80 operaciones.
- **15%:** Reconoce las dos uniones necesarias, distingue su efecto del filtro y evita un orden que omita el desempate temporal.
