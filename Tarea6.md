@echo off
chcp 65001 > nul
title 6 Tarea: Análisis Forense Post-Mortem de Bloqueo Masivo en Producción (Omnibank)

echo ==============================================================================
echo   6 TAREA: ANÁLISIS FORENSE POST-MORTEM DE BLOQUEO MASIVO EN PRODUCCIÓN
echo ==============================================================================
echo.

echo Bloqueo por ACCESS EXCLUSIVE:
echo ------------------------------------------------------------------------------
echo El incidente ocurrió porque un ALTER TABLE ADD CONSTRAINT CHECK normal
echo debe verificar las filas existentes. En una tabla con 75 millones de registros,
echo esto implica un escaneo prolongado. Además, esta operación requiere un bloqueo
echo ACCESS EXCLUSIVE que entra en conflicto con todos los demás modos de bloqueo,
echo incluyendo los utilizados para lecturas, inserciones y actualizaciones.
echo Por eso las operaciones concurrentes quedaron esperando hasta que el DBA
echo canceló la operación.
echo.

echo Corrección NOT VALID en dos fases:
echo ------------------------------------------------------------------------------
echo Para evitar repetir el incidente, primero se debe agregar el CHECK como NOT VALID.
echo PostgreSQL evita así el escaneo inmediato de las 75 millones de filas, pero la regla
echo comienza a aplicarse a las nuevas inserciones y actualizaciones.
echo.
echo ALTER TABLE core.transactions
echo ADD CONSTRAINT chk_transactions_description_valid
echo CHECK (LENGTH(TRIM(description)) >= 5)
echo NOT VALID;
echo.

echo Posteriormente, en una ventana controlada:
echo ------------------------------------------------------------------------------
echo ALTER TABLE core.transactions
echo VALIDATE CONSTRAINT chk_transactions_description_valid;
echo.

echo 3. Dictamen técnico:
echo ------------------------------------------------------------------------------
echo El error no fue el CHECK en sí, sino aplicarlo directamente sobre una tabla masiva
echo en producción. Para futuras migraciones de tablas críticas, Omnibank deberá utilizar
echo el patrón NOT VALID -> VALIDATE CONSTRAINT separando la protección de los datos
echo nuevos de la validación del historial existente.
echo.

pause