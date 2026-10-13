@echo off
chcp 65001 > nul
title Actividad 11: Transformaciones escalares y fechas de Omnibank

echo ==============================================================================
echo   ACTIVIDAD 11: TRANSFORMACIONES ESCALARES Y FECHAS DE OMNIBANK
echo ==============================================================================
echo.

echo [1. REPORTE DE LAS 5 CUENTAS Y SUS TITULARES]
echo SELECT
echo   a.account_id,
echo   a.account_number,
echo   c.customer_id,
echo   c.email,
echo   UPPER(CONCAT_WS(' ', c.first_name, c.last_name)) AS account_label,
echo   a.status,
echo   RIGHT(a.account_number, 4) AS account_number_mask
echo FROM core.accounts AS a
echo JOIN core.customers AS c ON c.customer_id = a.customer_id
echo ORDER BY a.account_id;
echo.

echo [2. REPORTE DE LOS 4 EVENTOS ANTERIORES AL CORTE INDICADO]
echo SELECT
echo   t.transactions_id,
echo   t.account_id,
echo   EXTRACT(YEAR FROM t.tx_timestamp)::Integer AS tx_year,
echo   EXTRACT(MONTH FROM t.tx_timestamp)::Integer AS tx_month,
echo   TO_CHAR(t.amount, 'FM999,999,990.00') AS amount_display,
echo   t.tx_timestamp AS original_timestamp,
echo   t.tx_timestamp + INTERVAL '15 Days' AS review_timestamp,
echo   TIMESTAMP WITH TIME ZONE '2026-10-01 00:00+00' AS tx_timestamp_cutoff
echo FROM core.transactions AS t
echo WHERE t.tx_timestamp ^< TIMESTAMPPTZ '2026-09-10 00:00+00'
echo ORDER BY t.transactions_id;
echo.

echo [3. CONTROLES DE RECUENTO DE LAS FUENTES]
echo SELECT COUNT(*) AS source_account_count
echo FROM core.accounts;
echo.
echo SELECT COUNT(*) AS source_event_count
echo FROM core.transactions
echo WHERE t.tx_timestamp ^< TIMESTAMPPTZ '2026-09-10 00:00+00';
echo.

pause