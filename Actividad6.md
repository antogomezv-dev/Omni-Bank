@echo off
chcp 65001 > nul
title 6 Actividad: Evolucionando el core bancario Omnibank con migraciones DDL seguras

echo ==============================================================================
echo   6 ACTIVIDAD: EVOLUCIONANDO EL CORE BANCARIO OMNIBANK CON MIGRACIONES DDL
echo ==============================================================================
echo.

echo CREATE TYPE core.loan_status_enum AS ENUM ('ACTIVE', 'PAID', 'DEFAULTED', 'CANCELLED');
echo.
echo BEGIN;
echo.
echo ALTER TABLE core.customers
echo ADD COLUMN credit_score INTEGER
echo ADD CONSTRAINT chk_customers_credit_score_range
echo CHECK (credit_score BETWEEN 300 AND 850);
echo.
echo ALTER TABLE core.accounts
echo ADD COLUMN iban VARCHAR(34) NULL
echo ADD CONSTRAINT uq_accounts_iban UNIQUE (iban);
echo.
echo ALTER TABLE core.loans
echo ALTER COLUMN status TYPE core.loan_status_enum
echo USING status::core.loan_status_enum;
echo.
echo COMMIT;
echo.

pause

Lo que aprendi:
Uso de subconsultas escalares y de tabla para realizar validaciones cruzadas entre cuentas y saldos consolidados 

FIX
# Day 06 Answer Key — Evolucionando el Core Bancario OmniBank con Migraciones DDL Seguras

## Expected Result

El estudiante confeccionará con impecable sintaxis un script de migración DDL (`migration_01_core_evolution.sql`) que integra dentro de un bloque transaccional indivisible (`BEGIN; ... COMMIT;`) sentencias precisas de `ALTER TABLE` para modificar las tablas en vivo `customers`, `accounts`, y `loans` en el esquema de OmniBank.

## Reference Solution

### 1. Script DDL Evolutivo Completo (`migration_01_core_evolution.sql`)

```sql
-- 1. Compilación del nuevo Tipo Enumerado en PostgreSQL para el ciclo de vida crediticio
CREATE TYPE core.loan_status_enum AS ENUM ('ACTIVE', 'PAID', 'DEFAULTED', 'CANCELLED');

-- 2. Inicio innegociable de transaccionalidad DDL en PostgreSQL
BEGIN;

-- Transformación 1: Añadir métrica de puntuación crediticia a los titulares bancarios con su constraint en duro
ALTER TABLE core.customers 
    ADD COLUMN credit_score INTEGER NULL;

ALTER TABLE core.customers 
    ADD CONSTRAINT chk_customers_credit_score_range 
    CHECK (credit_score BETWEEN 300 AND 850);

-- Transformación 2: Estandarización europea de identificación de cuentas bancarias y unicidad
ALTER TABLE core.accounts 
    ADD COLUMN iban VARCHAR(34) NULL;

ALTER TABLE core.accounts 
    ADD CONSTRAINT uq_accounts_iban UNIQUE (iban);

-- Transformación 3: Casteo estructural de columna de texto libre de préstamos a nuestro riguroso Enum catalogado
ALTER TABLE core.loans 
    ALTER COLUMN status DROP DEFAULT,
    ALTER COLUMN status TYPE core.loan_status_enum USING status::core.loan_status_enum,
    ALTER COLUMN status SET DEFAULT 'ACTIVE'::core.loan_status_enum;

-- Si ningún solo paso presentó divergencias en terminal o fallos operantes de conversión sintáctica:
COMMIT;
```

## Common Valid Variations

* En lugar de dividir el anexo de columna y la inclusión del constraint en dos cláusulas o mandatos `ALTER TABLE` por separado (p. ej. para `credit_score` o `iban`), redactarlos concatenados o en la misma línea declarativa resulta técnicamente equivalente:

```sql
ALTER TABLE core.customers 
    ADD COLUMN credit_score INTEGER CONSTRAINT chk_customers_credit_score CHECK (credit_score BETWEEN 300 AND 850);
```

* Para la traducción del campo `status` en `core.loans` al tipo Enum, utilizar la función explícita o casteo textual alterno como `USING CAST(status AS core.loan_status_enum)` es 100% equivalente al operador sintáctico doble dos puntos de PostgreSQL (`::`).

## Common Mistakes

* **Omitir el inicio (`BEGIN;`) y el final transaccional (`COMMIT;`):** Incide de plano contra el lineamiento primordial de protección del ejercicio, privando de resiliencia ante incidencias en caliente y desperdiciando la capacidad transaccional DDL de PostgreSQL por indolencia o descuido metodológico.
* **Intentar modificar el tipo del campo `status` sin la cláusula transitoria `USING`:** Compilar simplísticamente `ALTER COLUMN status TYPE core.loan_status_enum;` será rebatido drásticamente e impedido sin demora con el conocido error relacional *cannot be cast automatically* de Postgres.
