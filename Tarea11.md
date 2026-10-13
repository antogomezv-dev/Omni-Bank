@echo off
chcp 65001 > nul
title Tarea 11: Porcentaje de cuentas desactivadas y ventana de revisión temporal en Omnibank

echo ==============================================================================
echo   TAREA 11: PORCENTAJE DE CUENTAS DESACTIVADAS Y VENTANA DE REVISIÓN TEMPORAL
echo ==============================================================================
echo.

echo [1. PORCENTAJE DE CUENTAS DESACTIVADAS]
echo SELECT
echo   COUNT(*) AS total_cuentas,
echo   COUNT(*) FILTER (WHERE is_active = FALSE) AS cuentas_desactivadas,
echo   ROUND(
echo     (COUNT(*) FILTER (WHERE is_active = FALSE)::numeric * 100) /
echo     NULLIF(COUNT(*), 0), 2
echo   ) AS porcentaje_desactivadas
echo FROM core.accounts;
echo.

echo [2. ANTIGÜEDAD DE LOS 4 CUATRO EVENTOS AL CORTE DE REVISIÓN]
echo SELECT
echo   t.id AS event_id,
echo   t.tx_timestamp,
echo   TIMESTAMPTZ '2026-10-01 00:00+00' AS fecha_revision,
echo   AGE(
echo     TIMESTAMPTZ '2026-10-01 00:00+00',
echo     t.tx_timestamp
echo   ) AS antiguedad
echo FROM core.transactions AS t
echo ORDER BY t.tx_timestamp, t.id;
echo.

pause

Lo que aprendi 
Lo que aprendi fue el calculo de proporciones porcentuales con agregacion condicional FILTER (WHERE) y medicion de antiguedad operacional frente a fechas de corte

FIX
# Day 11 Answer Key — Porcentaje y revisión temporal

## Expected Result

La semilla tiene una cuenta desactivada de cinco, por lo que el porcentaje es 20.00%. Los cuatro movimientos conservan las fechas de revisión del 16 al 19 de septiembre.

## Reference Solution

```sql
SELECT COUNT(*) AS total_accounts,
       COUNT(*) FILTER (WHERE NOT is_active) AS inactive_accounts,
       ROUND(100.0 * COUNT(*) FILTER (WHERE NOT is_active)::numeric
             / NULLIF(COUNT(*), 0), 2) AS inactive_pct
FROM core.accounts;

SELECT CONCAT(UPPER(TRIM(c.first_name)), ' ', UPPER(TRIM(c.last_name))) AS customer_name,
       CONCAT('XXXX-', RIGHT(a.account_number, 4)) AS masked_account,
       t.transaction_id,
       t.tx_timestamp,
       t.tx_timestamp + INTERVAL '15 days' AS review_on,
       AGE(TIMESTAMPTZ '2026-10-01 00:00+00', t.tx_timestamp) AS age_at_review
FROM core.transactions t
JOIN core.accounts a ON a.account_id = t.account_id
JOIN core.customers c ON c.customer_id = a.customer_id
WHERE t.tx_timestamp < TIMESTAMPTZ '2026-09-10 00:00+00'
ORDER BY t.tx_timestamp;
```

La división entera 1/5 trunca a cero. El literal 100.0 y el cast `NUMERIC` conservan la fracción. El intervalo respeta la aritmética temporal de PostgreSQL; la fecha fija hace repetible la revisión.

**Lectura de la solución:** El responsable operativo solicita dos indicadores derivados de los reportes de la actividad: proporción de cuentas desactivadas y antigüedad de cada evento al corte de revisión. Ambos deben ser reproducibles con la semilla y mantener claro el tipo de dato de cada operación.

## Common Valid Variations

Se aceptan `CONCAT` o el operador de concatenación con tratamiento explícito de `NULL`, y otra fecha fija de corte si se recalculan coherentemente las expectativas.

## Common Mistakes

`CURRENT_DATE` vuelve variable un resultado que se presenta como reproducible; dividir enteros antes de convertir a `NUMERIC` puede truncar el porcentaje.

## Grading Rubric

* **35%:** Cuenta cuentas totales y desactivadas desde `accounts` y calcula el porcentaje con división numérica explícita; evita la división entera y muestra dos decimales.
* **30%:** Extiende el reporte temporal de la actividad con `AGE` o una diferencia de fechas respecto de un corte explícito. Mantén `tx_timestamp` y la fecha de revisión visibles para comparar el cálculo.
* **20%:** Comprueba el resultado con datos reproducibles: La semilla tiene una cuenta desactivada de cinco, por lo que el porcentaje es 20.00%. Los cuatro movimientos conservan las fechas de revisión del 16 al 19 de septiembre.
* **15%:** Reconoce y evita este error: `CURRENT_DATE` vuelve variable un resultado que se presenta como reproducible; dividir enteros antes de convertir a `NUMERIC` puede truncar el porcentaje.
