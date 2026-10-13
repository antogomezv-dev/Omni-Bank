@echo off
chcp 65001 > nul
title Tarea: Requerimientos No Funcionales de Omnibenk

echo ========================================================
echo   TAREA: REQUERIMIENTOS NO FUNCIONALES DE OMNIBENK
echo ========================================================
echo.

echo [NFR 1]
echo El sistema no puede generar números negativos
echo debe ser igual o mayor a un número entero.
echo.

echo [NFR 2]
echo Al momento de eliminar una cuenta, esta tendrá
echo que dejar un registro o marca llamada inactivo
echo para auditoría.
echo.

echo [NF 3]
echo Al realizar un cambio o modificación se señalará
echo fecha y hora exacta señalando el cambio exacto.
echo.

pause

LO QUE APRENDI:
Aprendi a identificar los Requerimientos No funcionles 

FIX 
# Day 01 Answer Key — Requerimientos No Funcionales de OmniBank

## Expected Result
El estudiante debe haber identificado las tres reglas de consistencia mencionadas en la actividad y el slack del CTO.

## Reference Solution
1. **NFR1 (Consistencia Matemática):** La base de datos debe impedir que las cuentas queden con saldos en números negativos.
2. **NFR2 (Auditoría / Soft Delete):** La base de datos debe impedir la eliminación física de un registro de cliente, y en su lugar debe poder marcarse como inactivo.
3. **NFR3 (Timestamps):** La base de datos debe registrar y actualizar automáticamente la fecha y hora exacta (`updated_at`) cada vez que el registro de una cuenta es modificado.

## Common Valid Variations
* "La tabla de clientes debe tener un campo booleano `is_active`." *(Esto ya es pensar en SQL, lo cual es muy bueno, aunque el requerimiento en lenguaje natural es suficiente).*

## Common Mistakes
* Confundir requerimientos funcionales con no funcionales. Por ejemplo, decir que "Un requerimiento es guardar el nombre del cliente", lo cual es Funcional (RF), no un Requerimiento No Funcional (NFR).
