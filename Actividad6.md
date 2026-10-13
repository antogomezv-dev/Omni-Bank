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