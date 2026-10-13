@echo off
chcp 65001 > nul
title 4 Tarea: Diseño de dominios customizados para protección financiera de Omnibank

echo ==============================================================================
echo   4 TAREA: DISEÑO DE DOMINIOS CUSTOMIZADOS PARA PROTECCIÓN FINANCIERA
echo ==============================================================================
echo.

echo 1. El dominio monetario bancario:
echo ------------------------------------------------------------------------------
echo CREATE DOMAIN "Monto_financiero" AS Numeric(15,2)
echo CHECK (value >= 0.00);
echo.

echo 2. Dominio de identificación fiscal / Cadena no vacía:
echo ------------------------------------------------------------------------------
echo CREATE DOMAIN "Texto_obligatorio" AS VARCHAR(100)
echo CHECK (value <> '' AND LENGTH(TRIM(value)) > 0);
echo.

pause