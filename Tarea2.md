@echo off
chcp 65001 > nul
title 2 Tarea: Refinamiento del ERD de Omnibank

echo ========================================================
echo   2 TAREA: REFINAMIENTO DEL ERD DE OMNIBANK
echo ========================================================
echo.

echo 1. Tablas y Campos:
echo    [Clientes]      : deleted_at
echo    [Cuentas]       : updated_at
echo    [Transacciones] : Trans_ID PK, UUID
echo.

echo 2. Llaves Primarias (PK):
echo    - Cliente_ID UUID PK
echo    - Cuenta_ID UUID PK
echo    - Trans_ID UUID PK
echo.
echo    Todas las llaves primarias (PK) utilizan UUID
echo    para generar identificadores aleatorios universales.
echo.

echo 3. Definicion de Entidades y Campos:
echo.
echo [Clientes]
echo    - Cliente_ID : UUID
echo    - Nombre     : No puede estar vacío
echo    - Correo     : Debe ser único
echo    - Número     : No puede estar vacío
echo    - Fecha Nac. : Obligatorio
echo    - deleted_at : Puede ser null
echo.

echo [Cuentas]
echo    - PK Cuenta_ID  : UUID
echo    - FK Cliente_ID : UUID
echo    - Usuario       : No puede estar vacío
echo    - Saldo         : No puede ser negativo
echo    - Tipo          : No puede estar vacío
echo    - updated_at    : Fecha de última actualización
echo.

echo [Transacciones]
echo    - PK Trans_ID   : UUID
echo    - FK Cuenta_ID  : UUID
echo    - Monto         : Debe ser mayor a 0
echo    - Tipo          : No puede estar vacío
echo    - Fecha         : Obligatoria
echo    - Registro      : Opcional
echo.

pause

Lo que aprendi:
Aprendi configuracion de migraciones en entornos de produccion mediante validaciones diferidas (NOT VALID y VALIDATE)para evitar el bloqueo masivos de tablas

FIX
# Day 02 Answer Key — Refinamiento del ERD de OmniBank

## Expected Result

El ERD base de la actividad debe haber sido actualizado para incluir los tipos de datos en las llaves (UUID en lugar de enteros), constraints anotados (ej. saldos no negativos, UNIQUE para el tax_id), y las nuevas columnas extraídas de los NFRs del Día 1.

## Reference Solution

### Esquema de Relaciones Refinado:

```
[Clientes] 1 ------------ N [Cuentas]
- cliente_id (PK, UUID)       - cuenta_id (PK, UUID)
- nombre                      - cliente_id (FK, UUID)
- correo                      - moneda
- telefono                    - tipo_cuenta
- fecha_nacimiento            - saldo_actual (CHECK: >= 0)
- tax_id (UNIQUE)             - limite_credito
- is_active (Soft Delete)     - updated_at (Timestamp NFR)

                   (Cuenta Origen)
[Cuentas] 1 ------------------------- N [Transacciones]
                                          - transaccion_id (PK, UUID)
                   (Cuenta Destino)       - from_account (FK, UUID -> Cuentas)
[Cuentas] 1 ------------------------- N   - to_account (FK, UUID -> Cuentas)
                                          - monto
                                          - tipo_transaccion
                                          - fecha_completada
```

## Common Valid Variations

* En lugar de `is_active`, el estudiante podría usar `deleted_at` (Timestamp), lo cual es otro patrón válido para Soft Deletes.

* Anotar el tipo de dato como `GUID` (término general) en lugar de `UUID` (término específico de Postgres).

## Common Mistakes

* **Error crítico:** Olvidar que si la Llave Primaria (PK) cambia a UUID, automáticamente todas las Llaves Foráneas (FK) que apunten a ella también deben ser UUID. No puedes vincular un UUID con un Entero.
