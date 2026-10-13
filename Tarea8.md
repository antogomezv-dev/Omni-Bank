@echo off
chcp 65001 > nul
title 8 Tarea: Auditoría Antifraude y Explicación de la Directiva (Omnibank)

echo ==============================================================================
echo   8 TAREA: AUDITORÍA ANTIFRAUDE Y EXPLICACIÓN DE LA DIRECTIVA (OMNIBANK)
echo ==============================================================================
echo.

echo [CONSULTA SQL]
echo SELECT
echo   Transaction_id,
echo   amount,
echo   tx_type,
echo   tx_timestamp,
echo   COALESCE(description, 'Sin descripción') AS descripcion_monitorada
echo FROM core.transactions
echo WHERE
echo   amount >= 10000.00
echo   AND tx_type IN ('transfer', 'withdrawal')
echo   OR description ILIKE '%%crypto%%'
echo   OR description ILIKE '%%offshore%%'
echo   OR description ILIKE '%%shell%%'
echo   OR description ILIKE '%%casino%%'
echo   OR NULLIF(TRIM(description), '') IS NULL
echo ORDER BY amount DESC;
echo.

echo [EXPLICACIÓN PARA LA DIRECTIVA]
echo La consulta anterior generaba falsos positivos porque AND tiene prioridad sobre OR,
echo haciendo que condiciones como description IS NULL fueran suficientes para activar la alerta,
echo incluso en depósitos normales.
echo La consulta corregida usa paréntesis para separar correctamente los dos bloques de riesgo
echo y exige que una descripción vacía o nula solo genere alerta cuando la operación sea
echo un depósito. Además, NULLIF(TRIM(description), '') detecta valores nulos, vacíos o espacios excesivos.
echo.

pause

Lo que aprendi:
Evaluacion de prioridad de operadores (AND/OR) uso explicito de parentesis y manejo de nulos/espacios (NULLIF, TRIM) para eliminar falsos positivos en depositos y retiros

FIX
# Day 08 Answer Key — Candidatos de Revisión

## Expected Result

Tres movimientos de revisión didáctica, sin atribuir fraude ni destinatario.

## Reference Solution

```sql
SELECT 
    transaction_id,
    account_id,
    tx_type,
    amount,
    COALESCE(NULLIF(TRIM(description), ''), 'SIN DESCRIPCIÓN') AS visible_description
FROM core.transactions
WHERE (tx_type IN ('TRANSFER', 'WITHDRAWAL') AND amount >= 100)
   OR (tx_type = 'WITHDRAWAL' AND NULLIF(TRIM(description), '') IS NULL)
ORDER BY amount DESC, transaction_id;
```

Los dos `TRANSFER` superan el umbral didáctico. El retiro de 50 entra por descripción faltante. El depósito no cumple ninguno de los dos bloques. Una alerta exploratoria requiere revisión adicional y no constituye una acusación ni un reporte legal.

## Common Valid Variations

* Se acepta `btrim` y una CTE que calcule la descripción normalizada antes de filtrar.

## Common Mistakes

* **Omitir paréntesis**: Cambia la precedencia de los operadores lógicos y altera los resultados esperados.
* **Tratar `description IS NULL` como prueba de fraude**: Confunde una observación incompleta o un dato faltante con una conclusión delictiva.
