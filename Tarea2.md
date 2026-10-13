@echo off
chcp 65001 > nul
title 2 Tarea: Refinamiento del ERD de Omnibank

echo ========================================================
echo   2 TAREA: REFINAMIENTO DEL ERD DE OMNIBANK
echo ========================================================
echo.

echo 1. Tablas y Campos:
echo    [Clientes]      : deleted_at
echo    [Cuentas]       : updated_at
echo    [Transacciones] : Trans_ID PK, UUID
echo.

echo 2. Llaves Primarias (PK):
echo    - Cliente_ID UUID PK
echo    - Cuenta_ID UUID PK
echo    - Trans_ID UUID PK
echo.
echo    Todas las llaves primarias (PK) utilizan UUID
echo    para generar identificadores aleatorios universales.
echo.

echo 3. Definicion de Entidades y Campos:
echo.
echo [Clientes]
echo    - Cliente_ID : UUID
echo    - Nombre     : No puede estar vacío
echo    - Correo     : Debe ser único
echo    - Número     : No puede estar vacío
echo    - Fecha Nac. : Obligatorio
echo    - deleted_at : Puede ser null
echo.

echo [Cuentas]
echo    - PK Cuenta_ID  : UUID
echo    - FK Cliente_ID : UUID
echo    - Usuario       : No puede estar vacío
echo    - Saldo         : No puede ser negativo
echo    - Tipo          : No puede estar vacío
echo    - updated_at    : Fecha de última actualización
echo.

echo [Transacciones]
echo    - PK Trans_ID   : UUID
echo    - FK Cuenta_ID  : UUID
echo    - Monto         : Debe ser mayor a 0
echo    - Tipo          : No puede estar vacío
echo    - Fecha         : Obligatoria
echo    - Registro      : Opcional
echo.

pause