@echo off
chcp 65001 > nul
title Actividad 3: Normalizando el prototipo monolítico de Omnibank

echo ====================================================================
echo   ACTIVIDAD 3: NORMALIZANDO EL PROTOTIPO MONOLÍTICO DE OMNIBANK
echo ====================================================================
echo.

echo 1. Primera forma Normal
echo --------------------------------------------------------------------
echo Cliente - nombre - y - contacto
echo Moneda - y - tipo
echo.
echo Tabla / Campos:
echo [Cliente] [Nombre] [contacto] [Moneda] [transacción] [Fecha] [Monto] [Sucursal] [dirección] [Cuenta]
echo.

echo 2. Segunda y tercera forma Normal
echo --------------------------------------------------------------------
echo 1.
echo   [Tabla Cliente]
echo     - Cliente_id PK
echo     - Nombre
echo     - contacto
echo     - Tipo de moneda
echo.
echo   [Tabla Transacción]
echo     - transacción_id PK
echo     - Monto
echo     - Cuenta
echo     - saldo
echo.
echo   Respuesta: Porque esos datos no son necesarios para la transacción, es decir requiere otros datos.
echo.
echo 2.
echo.

pause