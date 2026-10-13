@echo off
chcp 65001 > nul
title Actividad 2: Modelo Entidad Relación - Omnibenk

echo ========================================================
echo   ACTIVIDAD 2
echo ========================================================
echo.

echo [TABLAS Y ENTIDADES]
echo.
echo  +-----------------------+
echo  | Clientes              |
echo  +-----------------------+
echo  | Cliente-ID (PK)       |
echo  | Nombre                |
echo  | Correo                |
echo  | Número                |
echo  | Fecha de N.           |
echo  +-----------------------+
echo              |
echo              | (Relación 1:N / N:M)
echo              v
echo  +-----------------------+
echo  | Cuentas               |
echo  +-----------------------+
echo  | Cuentas_ID (PK)       |
echo  | Saldo                 |
echo  | Cantidad              |
echo  | Tipo                  |
echo  | Crédito               |
echo  +-----------------------+
echo              |
echo              | (Relación)
echo              v
echo  +-----------------------+
echo  | Transacciones         |
echo  +-----------------------+
echo  | Trans_ID (PK)         |
echo  | Monto                 |
echo  | Cantidad              |
echo  | Fecha                 |
echo  | Registro              |
echo  +-----------------------+
echo.

echo [REGLAS / NOTAS DE NEGOCIO]
echo 4: 1 cliente puede tener más de 1 cuenta.
echo    1 cuenta la puede manejar muchos clientes.
echo.

pause