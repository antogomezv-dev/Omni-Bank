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

Lo que aprendi:
Lo que aprendi fue el dominio de clausulas de seleccion (SELECT), proyeccion explicita de columnas y ordenamiento de registros operacionales 

FIX
# Day 04 Answer Key — Asignación de Tipos de Datos Nativos y Enums en OmniBank

## Expected Result
El estudiante compilará con éxito en su cliente local las tres declaraciones en firme de tipos enumerados (`ENUM`s) con sintaxis pura de PostgreSQL y corregirá el borrador incompleto del desarrollador, asignando `NUMERIC`, `TIMESTAMPTZ` y `UUID` en sus tablas relacionales.

## Reference Solution

### 1. Creación de Tipos Enumerados Bancarios (`ENUM`s)
```sql
-- Monedas admitidas
CREATE TYPE core.tipo_moneda AS ENUM ('USD', 'EUR', 'MXN', 'GBP');

-- Tipos de cuentas de clientes
CREATE TYPE core.tipo_cuenta AS ENUM ('CHECKING', 'SAVINGS', 'CREDIT');

-- Categorizaciones transaccionales permitidas
CREATE TYPE core.tipo_transaccion AS ENUM ('DEPOSIT', 'WITHDRAWAL', 'TRANSFER');
```
*(Nota: Incluir o no el esquema `core.` delante de los tipos es opcional en esta etapa inicial en tanto sean creados de forma consistente).*

### 2. Especificación Relacional Corregida (Diccionario y Campos)

* **Tabla Cuentas (Corrección de atributos críticos):**
  * `saldo_actual`: `NUMERIC(15, 2)` (Sustituye por completo al erróneo e impreciso `FLOAT(8)` con 15 dígitos de longitud en total y dos decimales bancarios inviolables y exactos).
  * `updated_at`: `TIMESTAMPTZ` (Provee protección en timestamps preservados bajo Tiempo Universal UTC).

* **Tipo de Llaves para Clientes, Cuentas y Transacciones:**
  * Todo atributo utilizado en función de Llaves Primarias (PK) y Llaves Foráneas (FK) portará inexorablemente el tipo nativo `UUID` de 128-bits. Su uso anula ataques de adivinanza o enumeración secuencial en los APIs de consulta del banco e incrementa el aislamiento distribuido.

## Common Valid Variations
* Integrar opciones monetarias locales adicionales dentro del `ENUM` según el país o zona del estudiante (por ejemplo, sumar `'COP'`, `'PEN'` o `'ARS'`).
* Optar por una escala mayor temporal (ej: `NUMERIC(18, 4)`) en caso de justificar que su versión bancaria procesa tasas interbancarias de criptomillonésimas de dólar o liquidaciones en bolsa, lo cual denotaría un análisis de arquitectura sobresaliente.

## Common Mistakes
* Mantener el uso de los tipos aproximados `REAL`, `FLOAT` o `DOUBLE PRECISION`: Incide directamente en el peor error transaccional financiero.
* Asignar `UUID` a las Llaves Primarias y conservar `INT` en sus respectivas Llaves Foráneas por descuido: PostgreSQL lanzará irremediablemente un error de tipo incompatible en el momento de crear el `CONSTRAINT FOREIGN KEY` al no empatar la estructura física hexadecimal frente a un entero convencional en 32 bits.
