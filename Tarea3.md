@echo off
chcp 65001 > nul
title Tarea 3: Análisis de dependencias transitivas y optimización consciente en Omnibank

echo ==============================================================================
echo   TAREA 3: ANÁLISIS DE DEPENDENCIAS TRANSITIVAS Y OPTIMIZACIÓN EN OMNIBANK
echo ==============================================================================
echo.

echo 1. Evaluación de la teoría:
echo ------------------------------------------------------------------------------
echo El auditor tiene razón en que el saldo actual es un dato derivado porque se
echo puede calcular a partir de todas las transacciones de la cuenta.
echo Por eso, desde una visión muy estricta de la normalización, guardar el saldo
echo puede considerarse información redundante. Sin embargo, esto no significa
echo necesariamente que el diseño esté mal, ya que en sistemas reales a veces se
echo guarda información calculada para mejorar el funcionamiento de las bases de datos.
echo.

echo 2. Problema de Rendimiento:
echo ------------------------------------------------------------------------------
echo Si una cuenta tiene 250,000 transacciones y calculamos el saldo con SUM()
echo cada vez que el cliente entra a la aplicación, PostgreSQL tendría que revisar
echo y sumar una gran cantidad de registros constantemente. Esto puede aumentar
echo el uso de CPU y la latencia, especialmente si muchos usuarios hacen consultas
echo al mismo tiempo. Por eso se mantiene saldo_actual como una desnormalización
echo controlada, para consultar el saldo de forma más rápida.
echo.

pause