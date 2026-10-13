@echo off
chcp 65001 > nul
title 8 Actividad: Extracción de Reportes Gerenciales en Omnibank y Detección de Vacios

echo ==============================================================================
echo   8 ACTIVIDAD: EXTRACCIÓN DE REPORTES GERENCIALES EN OMNIBANK Y DETECCIÓN
echo ==============================================================================
echo.

echo 1. Extracción:
echo ------------------------------------------------------------------------------
echo account_number
echo account_type
echo Balance
echo credit_limit
echo.
echo WHERE saldo monetario ^>= 1000
echo.
echo ORDER BY balance DESC
echo LIMIT 2 -- Muestra las primeras 2 filas más relevantes
echo.

echo SELECT Firstname, last_name, email
echo WHERE '%@email.com' ILIKE
echo From core.costumers
echo.

echo As description_visible
echo From core.transactions
echo Where description IS NULL
echo And tx_type = WITHRAMAL;
echo.

pause