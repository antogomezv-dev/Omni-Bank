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

Lo que aprendi:
Aprendi insercion de datos iniciales y escenarios de imponderables mediante el uso de patrones idempotentes con ON CONFLICT (UPSERT)

FIX
# Day 03 Answer Key — Normalizando el Prototipo Monolítico de OmniBank

## Expected Result

El estudiante deberá haber transformado el monolito de la tabla desordenada en un esquema relacional en 3NF consistente en 4 tablas: Clientes, Cuentas, Sucursales, y Transacciones, eliminando las redundancias de texto y datos multi-valorados y asegurando el uso de llaves primarias (PK) y llaves foráneas (FK) con dependencias transitivas resueltas.

## Reference Solution

### 1. Corrección a 1NF (Atomicidad de Campos):

* Desarmar `cliente_nombre_y_contacto` en columnas individuales: `nombre`, `email`, `telefono`.
* Desarmar `moneda_y_tipo` en: `moneda` y `tipo_cuenta`.

### 2. Separación Relacional en 3NF:

```
[Sucursales] (3NF - Elimina redundancia transitiva de direcciones repetidas en cada venta)
- sucursal_id (PK)      -- Ej: S-CDMX
- direccion             -- Ej: Av. Juárez #100

[Clientes] (3NF - Elimina duplicidad masiva del cliente en cada transacción o cuenta)
- cliente_id (PK/TaxID) -- Ej: TAX-9911 (O preferentemente UUID en práctica real)
- nombre                -- Ej: Roberto Soto
- email                 -- Ej: rob@mail.com
- telefono              -- Ej: 555-9011

[Cuentas] (3NF - Dependencia directa de Cuenta a Cliente y Sucursal de apertura)
- cuenta_numero (PK)    -- Ej: CTA-100
- cliente_id (FK -> Clientes)
- sucursal_id (FK -> Sucursales)
- moneda                -- Ej: USD
- tipo_cuenta           -- Ej: Checking
- saldo_actual          -- Ej: 1300.00

[Transacciones] (3NF - Registro atómico exclusivo del evento financiero)
- transaccion_id (PK)   -- Ej: TX-0001, TX-0002
- cuenta_numero (FK -> Cuentas)
- fecha_tx
- monto_tx
```

## Common Valid Variations

* Los estudiantes pueden elegir agregar una llave foránea de sucursal directamente sobre Transacciones (`sucursal_id_tx`) en caso de interpretar que los retiros y depósitos individuales se realizan en sucursales físicas diferentes a la sucursal original donde se abrió la cuenta bancaria. Esta es una solución perfectamente válida e incluso habitual en la banca transaccional distribuida (ATM/Cajas).
* Sustituir los códigos como `TAX-9911` y `CTA-100` por una columna explícita de identificador UUID para prepararse para la implementación en SQL de la semana entrante.
