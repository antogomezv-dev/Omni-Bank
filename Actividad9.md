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