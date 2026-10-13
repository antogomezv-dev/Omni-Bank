@echo off
chcp 65001 > nul
title Actividad 13: CTE y pipeline de métricas por cuenta en Omnibank

echo ==============================================================================
echo   ACTIVIDAD 13: CTE Y PIPELINE DE MÉTRICAS POR CUENTA EN OMNIBANK
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
echo     a.customer_id,
echo     a.currency,
echo     COALESCE(m.event_count, 0) AS event_count,
echo     COALESCE(m.cross_volume, 0) AS cross_volume
echo   FROM core.accounts AS a
echo   LEFT JOIN activity_by_account AS m
echo     ON m.account_id = a.account_id
echo   WHERE a.is_active = TRUE
echo ),
echo account_activity_report AS (
echo   SELECT
echo     aa.account_id,
echo     aa.account_number,
echo     c.full_name AS account_holder,
echo     aa.currency,
echo     aa.event_count,
echo     aa.cross_volume,
echo     CASE
echo       WHEN aa.event_count = 0 THEN 'Sin actividad'
echo       WHEN aa.event_count < 2 THEN 'Actividad baja'
echo       WHEN aa.event_count < 5 THEN 'Actividad media'
echo       ELSE 'Actividad alta'
echo     END AS activity_band
echo   FROM active_accounts_activity AS aa
echo   JOIN core.customers AS c
echo     ON c.customer_id = aa.customer_id
echo )
echo SELECT
echo   account_number,
echo   account_holder,
echo   currency,
echo   event_count,
echo   cross_volume,
echo   activity_band
echo FROM account_activity_report
echo ORDER BY account_number;
echo.

echo [NOTAS DE CONTROL]
echo Se utiliza LEFT JOIN: la cuenta se conserva aunque no tenga movimientos relacionados.
echo Después, COALESCE sustituye los valores NULL de los agregados por cero.
echo.

pause