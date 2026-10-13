@echo off
chcp 65001 > nul
title Actividad 10: Tablero de saldos y actividad registrada de Omnibank

echo ==============================================================================
echo   ACTIVIDAD 10: TABLERO DE SALDOS Y ACTIVIDAD REGISTRADA DE OMNIBANK
echo ==============================================================================
echo.

echo [1. TABLERO DE PRODUCTOS]
echo SELECT
echo   account_type,
echo   count(*) AS cantidad_cuentas,
echo   SUM(balance) AS saldo_total,
echo   MIN(balance) AS saldo_minimo,
echo   MAX(balance) AS saldo_maximo,
echo   AVG(balance) AS saldo_promedio
echo FROM core.accounts
echo WHERE is_active = true
echo GROUP BY account_type
echo ORDER BY saldo_total DESC, account_type;
echo.

echo [2. SEGMENTACIÓN DE TITULARES]
echo SELECT
echo   c.customer_id,
echo   c.name,
echo   c.email,
echo   COUNT(t.transaction_id) AS cantidad_movimientos,
echo   COALESCE(SUM(t.amount), 0) AS volumen_bruto
echo FROM core.customers c
echo LEFT JOIN core.accounts a ON c.customer_id = a.customer_id
echo LEFT JOIN core.transactions t ON a.account_id = t.account_id
echo GROUP BY c.customer_id, c.email, c.name
echo HAVING
echo   COUNT(t.transaction_id) >= 2
echo   OR COALESCE(SUM(t.amount), 0) > 100
echo ORDER BY volumen_bruto DESC, c.email;
echo.

echo [3. CONTROLES INDEPENDIENTES Y CTE]
echo WITH movimientos_por_cuenta AS (
echo   SELECT
echo     t.account_id,
echo     COUNT(t.transaction_id) AS cantidad_movimientos,
echo     COALESCE(SUM(t.amount), 0) AS volumen_bruto_registrado
echo   FROM core.transactions t
echo   GROUP BY t.account_id
echo )
echo SELECT
echo   c.customer_id,
echo   c.email,
echo   COALESCE(SUM(mpc.cantidad_movimientos), 0) AS cantidad_movimientos_control,
echo   COALESCE(SUM(mpc.volumen_bruto_registrado), 0) AS volumen_bruto_control
echo FROM core.customers c
echo LEFT JOIN core.accounts a ON a.customer_id = c.customer_id
echo LEFT JOIN movimientos_por_cuenta mpc ON mpc.account_id = a.account_id
echo GROUP BY c.customer_id, c.email
echo ORDER BY volumen_bruto_control DESC, c.email;
echo.

echo [NOTAS DE CONTROL]
echo - Reporte 1 (Grano = Tipo de cuenta activa): El saldo proviene directamente de core.accounts.balance, sin unirse a transactions.
echo - Reporte 2 (Grano = Cliente): La cantidad de movimientos y el volumen bruto provienen de core.transactions, enlazadas mediante account_id.
echo.

pause