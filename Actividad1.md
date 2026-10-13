@echo off
chcp 65001 > nul
title Actividad: Levantamiento de Requisitos de Omnibenk

color 0A
echo.
echo ===================================================
echo   1. ACTIVIDAD: LEVANTAMIENTO DE REQUISITOS OMNIBENK
echo ===================================================
echo.

echo ---------------------------------------------------
echo 1. ENTIDADES
echo ---------------------------------------------------
echo  - Clientes
echo  - Cuentas
echo  - Transacciones
echo.

echo ---------------------------------------------------
echo 2. REQUISITOS FUNCIONALES (RF - Registro)
echo ---------------------------------------------------
echo  [1] Almacen de Clientes:
echo      - Nombre, correo, numero, fecha de nacimiento,
echo        numero de identificacion fiscal.
echo.
echo  [2] Sistema de Cuentas Multiples:
echo      - Registro de numero de cuenta, moneda, tipo de cuenta,
echo        saldo actual y limite de credito.
echo.
echo  [3] Sistema de Transacciones:
echo      - Registrar cada vez que el dinero se mueva.
echo      - Cuenta origen, cuenta destino.
echo      - Guardar monto, tipo de transacción y fecha exacta.
echo.

echo ---------------------------------------------------
echo 3. RESTRICCIONES
echo ---------------------------------------------------
echo  - Numeros negativos por error del backend.
echo  - Fiscal no se puede repetir.
echo.
echo ===================================================
echo.

pause
```

He creado el archivo listo con la codificación de caracteres en español y una estructura clara para ser ejecutado directamente en la consola de comandos.