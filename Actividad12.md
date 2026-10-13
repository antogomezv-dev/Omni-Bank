@echo off
chcp 65001 > nul
title Actividad 12: Subconsultas correlacionadas y cuentas sin movimientos en Omnibank

echo ==============================================================================
echo   ACTIVIDAD 12: SUBCONSULTAS CORRELACIONADAS Y CUENTAS SIN MOVIMIENTOS
echo ==============================================================================
echo.

echo [1. MOVIMIENTOS CUYO IMPORTE SUPERA EL PROMEDIO DE SU PROPIA CUENTA]
echo SELECT
echo   t.transactions_id,
echo   t.account_id,
echo   t.amount,
echo   (
echo     SELECT AVG(t2.amount)
echo     FROM core.transactions AS t2
echo     WHERE t2.account_id = t.account_id
echo   ) AS account_average
echo FROM core.transactions AS t
echo WHERE t.amount ^> (
echo   SELECT AVG(t2.amount)
echo   FROM core.transactions AS t2
echo   WHERE t2.account_id = t.account_id
echo )
echo ORDER BY t.account_id, t.transactions_id;
echo.

echo [2. MEDIA AGRUPADA INDEPENDIENTE PARA CONTRASTAR EL PROMEDIO CORRELACIONADO]
echo SELECT
echo   account_id,
echo   AVG(amount) AS account_average_count,
echo   COUNT(*) AS event_count
echo FROM core.transactions
echo GROUP BY account_id
echo ORDER BY account_id;
echo.

echo [3. CUENTAS QUE NO TIENEN MOVIMIENTOS RELACIONADOS]
echo SELECT
echo   a.account_number,
echo   a.is_active
echo FROM core.accounts AS a
echo WHERE NOT EXISTS (
echo   SELECT 1
echo   FROM core.transactions AS t
echo   WHERE t.account_id = a.account_id
echo )
echo ORDER BY a.account_number;
echo.

echo [EXPLICACIÓN Y NOTAS]
echo Los movimientos de esta cuenta son de 150 y 50. Promedio = (150 + 50) / 2 = 100.
echo Únicamente el movimiento de 150 supera el promedio de su cuenta. El movimiento de 50 no lo supera.
echo.

pause