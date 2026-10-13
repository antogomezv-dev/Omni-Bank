@echo off
chcp 65001 > nul
title Actividad 4: Asignación de tipos de datos nativos y enums en Omnibank

echo ==============================================================================
echo   ACTIVIDAD 4: ASIGNACIÓN DE TIPOS DE DATOS NATIVOS Y ENUMS EN OMNIBANK
echo ==============================================================================
echo.

echo 1. Diseña los tipos enumerados (enums) del banco:
echo ------------------------------------------------------------------------------
echo - Create type Moneda As ENUM ('USD', EUR, MXN');
echo - Create type Cuentas_bancarias As ENUM... ('checking, savings, credit')
echo - Create type transacciones As ENUM... ('Deposit, withdrawal, transfer')
echo.

echo 2. Corrección de tipos nativos y especificación relacional:
echo ------------------------------------------------------------------------------
echo 1. Create type Saldo -> Numeric(p,s) / Decimal
echo 2. Create type Fecha -> Timestamptz
echo.
echo Tablas:
echo - Clientes ID : UUID PK
echo - Cuenta_ID   : UUID PK / Clientes ID : UUID FK
echo - Transacciones ID : UUID PK / Cuenta_ID : UUID FK
echo.

pause