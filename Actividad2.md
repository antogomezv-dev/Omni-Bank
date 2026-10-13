@echo off
chcp 65001 > nul
title Actividad 2: Modelo Entidad Relación - Omnibenk

echo ========================================================
echo   ACTIVIDAD 2
echo ========================================================
echo.

echo [TABLAS Y ENTIDADES]
echo.
echo  +-----------------------+
echo  | Clientes              |
echo  +-----------------------+
echo  | Cliente-ID (PK)       |
echo  | Nombre                |
echo  | Correo                |
echo  | Número                |
echo  | Fecha de N.           |
echo  +-----------------------+
echo              |
echo              | (Relación 1:N / N:M)
echo              v
echo  +-----------------------+
echo  | Cuentas               |
echo  +-----------------------+
echo  | Cuentas_ID (PK)       |
echo  | Saldo                 |
echo  | Cantidad              |
echo  | Tipo                  |
echo  | Crédito               |
echo  +-----------------------+
echo              |
echo              | (Relación)
echo              v
echo  +-----------------------+
echo  | Transacciones         |
echo  +-----------------------+
echo  | Trans_ID (PK)         |
echo  | Monto                 |
echo  | Cantidad              |
echo  | Fecha                 |
echo  | Registro              |
echo  +-----------------------+
echo.

echo [REGLAS / NOTAS DE NEGOCIO]
echo 4: 1 cliente puede tener más de 1 cuenta.
echo    1 cuenta la puede manejar muchos clientes.
echo.

pause

Lo que aprendi:
Configuracion e implementacion de scripts de creacion de esquemas DDL idempotentes y seguros en PostgreSQL

FIX
# Day 02 Answer Key — Diseñando el ERD Base de OmniBank

## Expected Result
El estudiante debe haber diseñado un diagrama ER con tres tablas conectadas correctamente mediante llaves foráneas. Las cardinalidades clave son: 1 Cliente -> Muchas Cuentas. Y la tabla Transacciones debe tener DOS llaves foráneas apuntando a Cuentas.

## Reference Solution

### Esquema de Relaciones:

```text
[Clientes] 1 ------------ N [Cuentas]
- cliente_id (PK)             - cuenta_id (PK)
- nombre                      - cliente_id (FK)
- correo                      - moneda
- telefono                    - tipo_cuenta
- fecha_nacimiento            - saldo_actual
- tax_id                      - limite_credito

                   (Cuenta Origen)
[Cuentas] 1 ------------------------- N [Transacciones]
                                          - transaccion_id (PK)
                   (Cuenta Destino)       - from_account (FK -> Cuentas)
[Cuentas] 1 ------------------------- N   - to_account (FK -> Cuentas)
                                          - monto
                                          - tipo_transaccion
                                          - fecha_completada
```

## Common Valid Variations
* Usar notación visual (diagramas de draw.io o Lucidchart) en lugar de texto.
* Nombrar las foráneas de transacciones de otra manera, ej. `cuenta_origen_id`, `cuenta_destino_id`.

## Common Mistakes
* **Error grave:** Conectar Clientes con Transacciones directamente. Las transacciones no pertenecen a un cliente en el aire, le pertenecen a una Cuenta de un cliente.
* **Error grave:** Tratar de meter la cuenta destino y origen en la tabla de cuentas, rompiendo la normalización y creando referencias circulares o N:M sin tabla intermedia.
* Olvidar las Llaves Primarias (PK).
