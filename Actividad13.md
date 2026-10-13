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

Lo que aprendi:
Aprendi la construccion de pipelines de metricas modulares organizando bloques de consulta con CTE y categorizacion condicional mediante CASE WHEN

FIX
# Day 13 Answer Key — CTE y pipeline de métricas por cuenta

## Expected Result

La salida incluye cuatro cuentas activas; ACCT-3002 aparece con cero eventos. El volumen total de la semilla es 800, distribuido sin multiplicar movimientos.

## Reference Solution

```
WITH movement AS (
  SELECT account_id,
         COUNT(*) AS tx_count,
         SUM(amount) AS gross_volume
  FROM core.transactions
  GROUP BY account_id
),
account_metrics AS (
  SELECT a.account_id,
         a.account_number,
         a.currency,
         a.customer_id,
         COALESCE(m.tx_count, 0) AS tx_count,
         COALESCE(m.gross_volume, 0) AS gross_volume
  FROM core.accounts a
  LEFT JOIN movement m USING (account_id)
  WHERE a.is_active
)
SELECT c.first_name,
       c.last_name,
       am.account_number,
       am.currency,
       am.tx_count,
       am.gross_volume,
       CASE 
         WHEN am.tx_count = 0 THEN 'SIN MOVIMIENTOS'
         ELSE 'CON MOVIMIENTOS'
       END AS activity_band
FROM account_metrics am
JOIN core.customers c USING (customer_id)
ORDER BY am.account_number;

SELECT (SELECT COUNT(*) FROM core.accounts WHERE is_active) AS active_accounts,
       (SELECT COALESCE(SUM(amount), 0) FROM core.transactions) AS direct_gross_volume;

```

**Lectura de la solución:** El tablero de productos necesita una medida de actividad por cuenta que otras consultas puedan reutilizar. El equipo no quiere repetir una agregación cada vez que agrega datos del titular o de la moneda. Un pipeline de CTE permite nombrar las etapas y declarar si cada una tiene una fila por movimiento, por cuenta o por moneda.

## Common Valid Variations

Se puede usar una subconsulta derivada por etapa si el grano y las filas conservadas son equivalentes; los nombres de las CTE no son parte del resultado.

## Common Mistakes

Agregar después de una unión que multiplica eventos o usar `INNER JOIN` antes de conservar cuentas vacías altera tanto el total como el denominador del promedio.

## Grading Rubric

* **35%:** Parte de `transactions` y construye una CTE con una fila por `account_id`: cuenta eventos y suma sus importes positivos como volumen bruto. No nombres saldo a esa suma.

* **30%:** En una segunda CTE conserva todas las cuentas activas con `LEFT JOIN` hacia la primera. Sustituye solo los agregados ausentes por cero y conserva `account_id` para controlar el grano.

* **20%:** Comprueba el resultado con datos reproducibles: La salida incluye cuatro cuentas activas; ACCT-3002 aparece con cero eventos. El volumen total de la semilla es 800, distribuido sin multiplicar movimientos.

* **15%:** Reconoce y evita este error: Agregar después de una unión que multiplica eventos o usar `INNER JOIN` antes de conservar cuentas vacías altera tanto el total como el denominador del promedio.
