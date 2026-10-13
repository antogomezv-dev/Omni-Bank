@echo off
chcp 65001 > nul
title 5 Tarea: Dictamen y Defensa de políticas de integridad referencial en omnibank

echo ==============================================================================
echo   5 TAREA: DICTAMEN Y DEFENSA DE POLÍTICAS DE INTEGRIDAD REFERENCIAL
echo ==============================================================================
echo.

echo Se rechaza el uso de ON DELETE CASCADE en el núcleo financiero de Omnibank,
echo ya que un error o eliminación accidental podría borrar automáticamente
echo información relacionada y causar una pérdida grave del historial bancario.
echo.

echo Las tablas core.trans y core.loans deben conservarse para garantizar la auditoría,
echo trazabilidad y cumplimiento fiscal. Cuando un cliente cierre su cuenta, o termine
echo su relación con el banco, no se deben borrar sus registros, se debe aplicar una
echo baja lógica usando "is_active = false o status = closed".
echo.

echo Es un sistema bancario, cerrar una cuenta significa desactivarla, no borrar su historial.
echo.

pause