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

Lo que aprendi:
Redaccion de subconsultas correlacionales para comparar transacciones individuales contra el promedio de su propia cuenta, y validaciones con NOT EXISTS

FIX
# Day 12 Answer Key — Subconsultas correlacionadas

## Expected Result

Un movimiento supera el promedio de su cuenta. ACCT-1002 y ACCT-3002 carecen de movimientos, aunque solo una está activa.

## Reference Solution

```
SELECT t_ext.transaction_id,
       t_ext.account_id,
       t_ext.amount,
       (SELECT ROUND(AVG(t_int.amount), 2)
        FROM core.transactions t_int
        WHERE t_int.account_id = t_ext.account_id) AS account_average
FROM core.transactions t_ext
WHERE t_ext.amount > (
  SELECT AVG(t_int.amount)
  FROM core.transactions t_int
  WHERE t_int.account_id = t_ext.account_id
)
ORDER BY t_ext.amount DESC;

SELECT account_id, ROUND(AVG(amount), 2) AS account_average
FROM core.transactions 
GROUP BY account_id 
ORDER BY account_id;

SELECT a.account_number, a.is_active, a.balance
FROM core.accounts a
WHERE NOT EXISTS (
  SELECT 1 FROM core.transactions t WHERE t.account_id = a.account_id
)
ORDER BY a.account_number;

```

**Lectura de la solución:** El equipo de revisión busca movimientos superiores al comportamiento habitual de su propia cuenta y cuentas que nunca han registrado eventos. Comparar un movimiento de John con el promedio global de todas las cuentas respondería otra pregunta. La ausencia de eventos también debe distinguirse del estado `is_active` para no emitir una alerta administrativa falsa.

## Common Valid Variations

Una CTE con promedio por `account_id` puede sustituir la subconsulta correlacionada si conserva la misma comparación y el tratamiento de cuentas sin eventos.

## Common Mistakes

Olvidar la correlación por `account_id` convierte una excepción local en una comparación global; `NOT IN` con `NULL` puede eliminar resultados esperados.

## Grading Rubric

* **35%:** Calcula para cada movimiento el promedio de `amount` de los eventos que comparten su `account_id`. Usa una subconsulta correlacionada o una estrategia equivalente y conserva el identificador de la fila externa.

* **30%:** Filtra únicamente movimientos cuyo importe supera ese promedio. Comprueba el resultado contra un promedio agrupado independiente antes de interpretar el evento.

* **20%:** Comprueba el resultado con datos reproducibles: Un movimiento supera el promedio de su cuenta. ACCT-1002 y ACCT-3002 carecen de movimientos, aunque solo una está activa.

* **15%:** Reconoce y evita este error: Olvidar la correlación por `account_id` convierte una excepción local en una comparación global; `NOT IN` con `NULL` puede eliminar resultados esperados.
