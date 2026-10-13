@echo off
chcp 65001 > nul
title Actividad 14: SELF JOIN, matriz de productos y anti JOIN en Omnibank

echo ==============================================================================
echo   ACTIVIDAD 14: SELF JOIN, MATRIZ DE PRODUCTOS Y ANTI JOIN EN OMNIBANK
echo ==============================================================================
echo.

echo [1. PAREJAS DE EVENTOS REALES DE UNA MISMA CUENTA]
echo SELECT
echo   a.account_number,
echo   c.full_name AS account_holder,
echo   t1.transactions_id AS event_1_id,
echo   t1.created_at AS event_1_at,
echo   t1.transactions_type AS event_1_type,
echo   t1.amount AS event_1_amount,
echo   t2.transactions_id AS event_2_id,
echo   t2.created_at AS event_2_at,
echo   t2.transactions_type AS event_2_type,
echo   t2.amount AS event_2_amount
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
echo ORDER BY
echo   a.account_number,
echo   t1.created_at,
echo   t2.created_at;
echo.

pause