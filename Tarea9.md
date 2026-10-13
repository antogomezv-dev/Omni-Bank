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