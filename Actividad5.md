@echo off
chcp 65001 > nul
title 5 Actividad: Levantando los cimientos del Core Bancario Omnibank (DDC)

echo ==============================================================================
echo   5 ACTIVIDAD: LEVANTANDO LOS CIMIENTOS DEL CORE BANCARIO OMNIBANK (DDC)
echo ==============================================================================
echo.

echo 1. Espacio Modular:
echo ------------------------------------------------------------------------------
echo CREATE SCHEMA IF NOT EXISTS core;
echo.

echo 2. Construyendo la entidad raiz:
echo ------------------------------------------------------------------------------
echo CREATE TABLE core.customers (
echo     Clientes_del_banco UUID DEFAULT gen_random_uuid(),
echo     nombre_completo VARCHAR(100) UNIQUE,
echo     Email VARCHAR(150) UNIQUE,
echo     Numero VARCHAR(n) NOT NULL,
echo     created_at Fecha TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP NOT NULL,
echo     is_active BOOLEAN DEFAULT true
echo );
echo.

echo 3. Construyendo Entidades Hijas:
echo ------------------------------------------------------------------------------
echo Create table core.accounts (
echo     Cliente_del_banco UUID DEFAULT gen_random_uuid(),
echo     customer_id FK,
echo     Create core.Tipo_moneda NOT NULL,
echo     Domain_Monto_financiero AS Numeric(15,2)
echo );
echo.

pause