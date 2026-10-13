@echo off
chcp 65001 > nul
title 7 Actividad: Población inicial del core Bancario Omnibank (DMC Idempotente)

echo ==============================================================================
echo   7 ACTIVIDAD: POBLACIÓN INICIAL DEL CORE BANCARIO OMNIBANK (DMC IDEMPOTENTE)
echo ==============================================================================
echo.

echo 1. Clientes pioneros:
echo ------------------------------------------------------------------------------
echo INSERT INTO core.customers
echo (id, first_name, last_name, email, phone, tax_id, date_of_birth)
echo ...
echo ON CONFLICT (email) DO NOTHING;
echo.

echo 2. Cuentas bancarias:
echo ------------------------------------------------------------------------------
echo INSERT INTO core.accounts
echo (id, customer_id, account_number, account_type, balance, credit_limit)
echo ...
echo ON CONFLICT (account_number) DO NOTHING;
echo.

echo 3. Primeras transacciones:
echo ------------------------------------------------------------------------------
echo INSERT INTO core.transactions
echo (id, from_account_id, to_account_id, amount, type, status, description)
echo ...
echo.

echo 4. Deducción de comisión y confirmación:
echo ------------------------------------------------------------------------------
echo UPDATE core.accounts
echo SET balance = balance - 50.00
echo WHERE account_number = 'ACCT-1001'
echo RETURNING account_number, balance;
echo.
echo COMMIT;
echo.

pause