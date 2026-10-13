@echo off
chcp 65001 > nul
title Actividad 14: SELF JOIN, Matriz de Productos y Anti JOIN en Omnibank

echo ==============================================================================
echo   ACTIVIDAD 14: SELF JOIN, MATRIZ DE PRODUCTOS Y ANTI JOIN EN OMNIBANK
echo ==============================================================================
echo.

echo [1. PAREJAS DE EVENTOS REALES DE UNA MISMA CUENTA (FILTRO POR CLIENTE 'John')]
echo SELECT
echo   a.account_number,
echo   t1.transactions_id AS event_1_id,
echo   t2.transactions_id AS event_2_id,
echo   t1.created_at AS event_1_at,
echo   t2.created_at AS event_2_at,
echo   t2.created_at - t1.created_at AS elapsed_time
echo FROM core.transactions AS t1
echo JOIN core.transactions AS t2
echo   ON t1.account_id = t2.account_id
echo   AND t1.transactions_id ^< t2.transactions_id
echo   AND t2.created_at ^> t1.created_at
echo   AND t2.created_at ^< t1.created_at + INTERVAL '48 hours'
echo JOIN core.accounts AS a
echo   ON a.account_id = t1.account_id
echo JOIN core.customers AS c
echo   ON c.customer_id = a.customer_id
echo WHERE c.full_name ILIKE 'John%%'
echo ORDER BY
echo   a.account_number,
echo   t1.created_at,
echo   t2.created_at;
echo.

echo [MATRIZ DE POSIBILIDADES CLIENTE - TIPO]
echo (Configuración analítica para cruce multidimensional de transacciones y perfiles)
echo.

pause

Lo que aprendi:
Aprendi el analisis avanzado de series temporales acopladas mediante SELF JOIN y filtros por cliente para la identificacion de conductas atipaticas o potencialmente fraudulentas 

FIX
# Day 14 Answer Key — Extensión de Self JOIN, matriz de productos y anti JOIN

## Expected Result
Tres cuentas activas carecen de DEPOSIT: ACCT-1001, ACCT-2001 y ACCT-3002. Las dos primeras sí tienen otros movimientos.

## Reference Solution
```sql
SELECT a.account_number, a.account_type
FROM core.accounts a
LEFT JOIN core.transactions t ON t.account_id = a.account_id AND t.tx_type = 'DEPOSIT'
WHERE a.is_active AND t.transaction_id IS NULL
ORDER BY a.account_number;
