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

LO QUE APRENDI:
Comprension de Requerimientos Funcionales y No Funcionales

FIX

# Day 01 Answer Key — Levantamiento de Requisitos de OmniBank

## Expected Result
El estudiante debe haber identificado las tres entidades principales mencionadas en el texto y sus campos mínimos, así como la restricción de que el Tax ID no puede repetirse.

## Reference Solution

### 1. Lista de Entidades:
* Clientes (`Customers`)
* Cuentas (`Accounts`)
* Transacciones (`Transactions`)

### 2. Requerimientos Funcionales (RF):
* **RF1:** El sistema debe almacenar de los Clientes: nombre, correo, teléfono, fecha de nacimiento y Tax ID.
* **RF2:** El sistema debe almacenar de las Cuentas: número de cuenta, moneda, tipo de cuenta, saldo actual y límite de crédito, y a qué cliente pertenecen.
* **RF3:** El sistema debe almacenar de las Transacciones: cuenta origen, cuenta destino, monto, tipo de transacción y fecha/hora exacta.

### 3. Restricción de unicidad:
El sistema no debe permitir que dos clientes tengan el mismo Tax ID o número de identificación fiscal (Debe ser `UNIQUE`).

---

## Common Valid Variations
Los estudiantes pueden agregar "correo electrónico" como otra restricción de unicidad, lo cual es muy válido en sistemas reales, aunque el texto se enfoca en el Tax ID.

## Common Mistakes
* Extraer entidades como "Dinero" o "Bancos", las cuales no son tablas que vamos a modelar.
* Tratar de escribir los nombres en formato SQL (`CREATE TABLE clientes...`) en lugar de extraer el concepto del negocio en lenguaje natural.
