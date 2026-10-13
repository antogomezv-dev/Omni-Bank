@echo off
chcp 65001 > nul
title Tarea 13: Extensión de CTE y Pipeline de Métricas por Cuenta en Omnibank

echo ==============================================================================
echo   TAREA 13: EXTENSIÓN DE CTE Y PIPELINE DE MÉTRICAS POR CUENTA EN OMNIBANK
echo ==============================================================================
echo.

echo WITH activity_by_account AS (
echo   SELECT
echo     t.account_id,
echo     COUNT(*) AS event_count,
echo     SUM(t.amount) AS cross_volume
echo   FROM core.transactions AS t
echo   GROUP BY t.account_id
echo ),
echo active_accounts_activity AS (
echo   SELECT
echo     a.account_id,
echo     a.account_number,
echo     a.currency,
echo     COALESCE(m.event_count, 0) AS event_count,
echo     COALESCE(m.cross_volume, 0) AS gross_volume
echo   FROM core.accounts AS a
echo   LEFT JOIN activity_by_account AS m
echo     ON m.account_id = a.account_id
echo   WHERE a.is_active = TRUE
echo ),
echo currency_activity_summary AS (
echo   SELECT
echo     currency,
echo     COUNT(*) AS account_count,
echo     SUM(gross_volume) AS gross_volume,
echo     ROUND(AVG(gross_volume), 2) AS average_gross_volume_per_account
echo   FROM active_accounts_activity
echo   GROUP BY currency
echo )
echo SELECT
echo   currency,
echo   account_count,
echo   gross_volume,
echo   average_gross_volume_per_account
echo FROM currency_activity_summary
echo ORDER BY currency;
echo.

echo [RESULTADO / TABLA DE CONTROL]
echo Moneda ^| Cuentas Activas ^| Volumen Bruto ^| Promedio
echo USD    ^| 4               ^| 800           ^| 200
echo.

pause

Lo que aprendi 
Aprendi la extension de arquitectura CTE para resumir y agregar volumenes operacionales brutos segmentados por tipo de divisa 

FIX
# Day 13 Answer Key — Extensión de CTE y pipeline de métricas por cuenta

## Expected Result
La semilla inicial produce una fila USD, cuatro cuentas activas, volumen bruto 800 y promedio de 200 por cuenta.

## Reference Solution
```sql
WITH movement AS (
 SELECT account_id, SUM(amount) AS gross_volume
 FROM core.transactions GROUP BY account_id
), account_metrics AS (
 SELECT a.account_id, a.currency, COALESCE(m.gross_volume,0) AS gross_volume
 FROM core.accounts a LEFT JOIN movement m USING (account_id)
 WHERE a.is_active
), by_currency AS (
 SELECT currency, COUNT(*) AS account_count, SUM(gross_volume) AS gross_volume
 FROM account_metrics GROUP BY currency
)
SELECT currency, account_count, gross_volume,
       ROUND(gross_volume / account_count,2) AS average_per_account
FROM by_currency ORDER BY currency;

SELECT (SELECT COUNT(*) FROM core.accounts WHERE is_active) AS active_accounts,
       (SELECT COALESCE(SUM(amount),0) FROM core.transactions) AS direct_gross_volume,
       (SELECT COUNT(*) FROM core.transactions t JOIN core.accounts a USING (account_id)
        WHERE a.account_number='ACCT-3002') AS empty_account_events;
