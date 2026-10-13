@echo off
chcp 65001 > nul
title Tarea 12: Exclusión segura con NOT EXISTS y NULL en Omnibank

echo ==============================================================================
echo   TAREA 12: EXCLUSIÓN SEGURA CON NOT EXISTS Y NULL EN OMNIBANK
echo ==============================================================================
echo.

echo [1. CUENTAS ACTIVAS SIN NINGÚN MOVIMIENTO]
echo SELECT
echo   a.account_id
echo FROM core.accounts AS a
echo WHERE a.is_active = TRUE
echo   AND NOT EXISTS (
echo     SELECT 1
echo     FROM core.transactions AS t
echo     WHERE t.account_id = a.account_id
echo   )
echo ORDER BY a.account_id;
echo.

echo [2. CONTEO DE CUENTAS ACTIVAS SIN EVENTOS]
echo SELECT
echo   COUNT(*) AS active_accounts_without_events
echo FROM core.accounts AS a
echo WHERE a.is_active = TRUE
echo   AND NOT EXISTS (
echo     SELECT 1
echo     FROM core.transactions AS t
echo     WHERE t.account_id = a.account_id
echo   );
echo.

echo [3. CUENTAS SIN DEPÓSITOS]
echo SELECT
echo   COUNT(*) AS accounts_without_deposit
echo FROM core.accounts AS a
echo WHERE NOT EXISTS (
echo     SELECT 1
echo     FROM core.transactions AS t
echo     WHERE t.account_id = a.account_id
echo       AND t.transactions_type = 'DEPOSIT'
echo   );
echo.

echo [4. PRUEBA DE COMPORTAMIENTO CON VALORES NULL (NOT IN vs NOT EXISTS)]
echo WITH sample_values AS (
echo   SELECT unnest(ARRAY[2, 3, NULL])::integer[] AS value
echo )
echo SELECT
echo   9 NOT IN (SELECT value FROM sample_values) AS not_in_result,
echo   NOT EXISTS (
echo     SELECT 1
echo     FROM sample_values
echo     WHERE value = 3
echo   ) AS not_exists_result;
echo.

pause