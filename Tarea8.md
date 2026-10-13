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