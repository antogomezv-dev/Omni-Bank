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