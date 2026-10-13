@echo off
chcp 65001 > nul
title 7 Tarea: Sincronización eficiente del Pipeline KYC con escrituras idempotentes

echo ==============================================================================
echo   7 TAREA: SINCRONIZACIÓN EFICIENTE DEL PIPELINE KYC CON ESCRITURAS IDEMPOTENTES
echo ==============================================================================
echo.

echo El problema del sistema KYC era que el backend utilizaba el patrón SELECT + IF /
echo ELSE + INSERT / UPDATE para comprobar si cada cliente existía. Esto genera mucho
echo tráfico entre la aplicación y PostgreSQL porque se realizan varias consultas por cada
echo registro. Con 1 millón de registros, el número de operaciones y viajes de red aumenta
echo considerablemente, provocando que el proceso tarde horas.
echo.

echo La solución es utilizar un UPSERT mediante INSERT ON CONFLICT DO UPDATE. PostgreSQL
echo se encarga directamente de decidir si debe insertar un cliente nuevo o actualizar uno
echo existente, evitando la consulta previa. Para los clientes existentes solamente se actualizan
echo nombre, apellido y teléfono mientras que customer_id y created_at permanecen intactas.
echo.

echo SQL:
echo ------------------------------------------------------------------------------
echo INSERT INTO core.customers
echo (first_name, last_name, email, phone, number)
echo VALUES ($1, $2, $3, $4)
echo ON CONFLICT (email)
echo DO UPDATE SET
echo First_name = EXCLUDED.First_name,
echo Last_name = EXCLUDED.Last_name,
echo Phone_number = EXCLUDED.phone_number,
echo RETURNING customer_id, email;
echo.

pause