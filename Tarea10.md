@echo off
chcp 65001 > nul
title Tarea: Promedios con NULL y reporte de actividad de crédito en Omnibank

echo ==============================================================================
echo   TAREA: PROMEDIOS CON NULL Y REPORTE DE ACTIVIDAD DE CRÉDITO (OMNIBANK)
echo ==============================================================================
echo.

echo [1. CONSULTA DE PROMEDIOS CON Y SIN COALESCE]
echo WITH muestra AS (
echo   SELECT *
echo   FROM (VALUES
echo     (10000::numeric),
echo     (0::numeric),
echo     (0::numeric),
echo     (NULL::numeric)
echo   ) AS v(valor)
echo )
echo SELECT
echo   AVG(valor) AS promedio_sin_coalesce,
echo   COUNT(valor) AS denominador_3
echo FROM muestra;
echo.
echo WITH muestra AS (
echo   SELECT *
echo   FROM (VALUES
echo     (10000::numeric),
echo     (0::numeric),
echo     (0::numeric),
echo     (NULL::numeric)
echo   ) AS v(valor)
echo )
echo SELECT
echo   AVG(COALESCE(valor, 0)) AS promedio_con_coalesce,
echo   COUNT(*) AS denominador_4
echo FROM muestra;
echo.

echo [2. REPORTE DE ACTIVIDAD DE CRÉDITO]
echo SELECT
echo   a.account_number,
echo   a.balance AS saldo_almacenado,
echo   COUNT(t.transactions_id) AS numero_movimientos,
echo   COALESCE(SUM(t.amount), 0) AS volumen_bruto_registrado
echo FROM core.accounts AS a
echo LEFT JOIN core.transactions AS t
echo   ON t.account_id = a.account_id
echo WHERE a.account_type = 'CREDIT'
echo   AND a.is_active = TRUE
echo GROUP BY
echo   a.account_id,
echo   a.account_number,
echo   a.balance
echo HAVING
echo   COUNT(t.transactions_id) >= 0
echo   OR COALESCE(SUM(t.amount), 0) >= 300
echo ORDER BY
echo   a.account_number;
echo.

pause