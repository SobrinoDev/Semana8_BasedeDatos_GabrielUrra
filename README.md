# Semana 8 - Base de Datos: Construyendo una Base de Datos a partir de un Modelo Relacional Normalizado (Taller Mecánico Mikes Ltda.)

Actividad sumativa individual desarrollada con **Oracle SQL Developer** y **SQL Developer Data Modeler**. Se implementa un Modelo Relacional normalizado mediante sentencias DDL (creación de tablas y restricciones), se incorporan nuevas reglas de negocio con `ALTER TABLE`, se pueblan las tablas usando `IDENTITY` y objetos `SEQUENCE`, y finalmente se generan informes con la sentencia `SELECT`.

## Contexto de negocio

El taller **Mikes Ltda.**, con presencia en diversas ciudades del norte de Chile, ofrece a sus clientes servicios multimarca: cambios de aceite, reparación de frenos, diagnóstico de motores, desabolladura, pintura, instalación de complementos, mecánica general, mantenimiento preventivo y mecánica de alto nivel. Cada mantención es atendida por un mecánico especializado, se realiza en una sucursal y se desglosa en distintos servicios. Los clientes se clasifican en **estándar** y **premium**. A partir del Modelo Relacional entregado se construyó el script completo, organizado en cuatro casos:

| Caso | Descripción |
|---|---|
| Caso 1 | Implementación del modelo: creación de tablas (de las más fuertes a las más débiles) con sus restricciones PK y FK. |
| Caso 2 | Modificación del modelo con `ALTER TABLE` para incorporar nuevas reglas de negocio (cambio de PK, UN y CK). |
| Caso 3 | Poblamiento de las tablas PAIS, CIUDAD, SUCURSAL, SERVICIO, MECANICO y MANTENCION usando `IDENTITY` y `SEQUENCE`. |
| Caso 4 | Recuperación de datos: dos informes de simulación sobre los sueldos de los mecánicos. |

## Contenido del repositorio

| Archivo / carpeta | Descripción |
|---|---|
| `Semana8_BasedeDatos_GabrielUrra.sql` | Script completo (Casos 1 al 4), listo para ejecutar con F5 en SQL Developer. |
| `Semana8_BasedeDatos_GabrielUrra.dmd` | Diseño del modelo en SQL Developer Data Modeler. |
| `Semana8_BasedeDatos_GabrielUrra/` | Carpeta de datos del diseño (necesaria para abrir el `.dmd`). |

## Entidades identificadas

Se implementaron **14 tablas**: PAIS, SERVICIO, MARCA, TIPO_AUTOMOVIL, CLIENTE y MECANICO como tablas fuertes; CIUDAD y SUCURSAL, que describen la ubicación geográfica de cada sucursal; MODELO, que depende de MARCA mediante una clave primaria compuesta; ESTANDAR y PREMIUM como subtipos de CLIENTE; AUTOMOVIL, que relaciona al cliente con su vehículo; MANTENCION como entidad central, con MECANICO incluyendo una relación recursiva para identificar a su supervisor; y DETALLE_SERVICIO como entidad asociativa que resuelve la relación N:M entre MANTENCION y SERVICIO.

### PAIS
| Atributo | Tipo | Clave |
|---|---|---|
| id_pais | NUMBER(3) IDENTITY (inicia en 9, incrementa de 3 en 3) | PK |
| nom_pais | VARCHAR2(30) | Obligatorio |

### CIUDAD
| Atributo | Tipo | Clave |
|---|---|---|
| id_ciudad | NUMBER(3) — poblado con SEQ_CIUDAD (inicia en 165, incrementa de 5 en 5) | PK |
| nom_ciudad | VARCHAR2(30) | Obligatorio |
| cod_pais | NUMBER(3) | FK → PAIS |

### SUCURSAL
| Atributo | Tipo | Clave |
|---|---|---|
| id_sucursal | CHAR(3) | PK |
| nom_sucursal | VARCHAR2(20) | Obligatorio |
| calle | VARCHAR2(20) | Obligatorio |
| num_calle | NUMBER(4) | Obligatorio |
| cod_ciudad | NUMBER(3) | FK → CIUDAD |

### SERVICIO
| Atributo | Tipo | Clave |
|---|---|---|
| id_servicio | NUMBER(3) — poblado con SEQ_SERVICIO (inicia en 400, incrementa de 2 en 2) | PK |
| descripcion | VARCHAR2(100) | Obligatorio |
| costo | NUMBER(7) | Obligatorio |

### MARCA
| Atributo | Tipo | Clave |
|---|---|---|
| id_marca | NUMBER(2) | PK |
| descripcion | VARCHAR2(20) | Obligatorio |

### MODELO
| Atributo | Tipo | Clave |
|---|---|---|
| id_modelo | NUMBER(5) | PK |
| marca_id | NUMBER(2) | PK / FK → MARCA |
| descripcion | VARCHAR2(20) | Obligatorio |

### TIPO_AUTOMOVIL
| Atributo | Tipo | Clave |
|---|---|---|
| id_tipo | CHAR(3) | PK |
| descripcion | VARCHAR2(20) | Obligatorio |

### CLIENTE
| Atributo | Tipo | Clave |
|---|---|---|
| rut | NUMBER(8) | PK |
| dv | CHAR(1) — CHECK (0-9, K) | Obligatorio |
| pnombre | VARCHAR2(20) | Obligatorio |
| snombre | VARCHAR2(20) | Opcional |
| apaterno | VARCHAR2(20) | Obligatorio |
| amaterno | VARCHAR2(20) | Obligatorio |
| telefono | VARCHAR2(12) | Opcional |
| email | VARCHAR2(40) | Opcional (único) |
| tipo_cli | CHAR(1) | Obligatorio |

### ESTANDAR (subtipo de CLIENTE)
| Atributo | Tipo | Clave |
|---|---|---|
| cl_rut | NUMBER(8) | PK / FK → CLIENTE |
| puntaje_fidelidad | NUMBER(10) | Obligatorio |

### PREMIUM (subtipo de CLIENTE)
| Atributo | Tipo | Clave |
|---|---|---|
| cl_rut | NUMBER(8) | PK / FK → CLIENTE |
| pesos_clientes | NUMBER(10) | Obligatorio |
| monto_credito | NUMBER(10) | Opcional |

### AUTOMOVIL
| Atributo | Tipo | Clave |
|---|---|---|
| patente | CHAR(8) | PK |
| anio | NUMBER(4) | Obligatorio |
| cant_puertas | NUMBER(1) | Obligatorio |
| km | NUMBER(6) | Obligatorio |
| color | VARCHAR2(30) | Obligatorio |
| cod_tipo_auto | CHAR(3) | FK → TIPO_AUTOMOVIL |
| cod_modelo | NUMBER(5) | FK → MODELO |
| cod_marca | NUMBER(2) | FK → MODELO |
| cl_rut | NUMBER(8) | FK → CLIENTE |

### MECANICO
| Atributo | Tipo | Clave |
|---|---|---|
| cod_mecanico | NUMBER(5) IDENTITY (inicia en 460, incrementa de 7 en 7) | PK |
| pnombre | VARCHAR2(20) | Obligatorio |
| snombre | VARCHAR2(20) | Opcional |
| apaterno | VARCHAR2(20) | Obligatorio |
| amaterno | VARCHAR2(20) | Obligatorio |
| bono_jefatura | NUMBER(10) | Opcional |
| sueldo | NUMBER(10) — CHECK (>= 510.000) | Obligatorio |
| monto_impuestos | NUMBER(10) | Obligatorio |
| cod_supervisor | NUMBER(5) | FK → MECANICO (recursiva, opcional) |

### MANTENCION
| Atributo | Tipo | Clave |
|---|---|---|
| num_mantencion | NUMBER(4) | PK |
| cod_sucursal | CHAR(3) | PK / FK → SUCURSAL |
| fecha_ingreso | DATE | Obligatorio |
| fecha_salida | DATE | Opcional |
| patente_auto | CHAR(8) | FK → AUTOMOVIL (opcional) |
| cod_mecanico | NUMBER(5) | FK → MECANICO |
| estado | VARCHAR2(15) — CHECK (Reserva, Ingresado, Entregado, Anulado) | Opcional |

### DETALLE_SERVICIO (entidad asociativa)
| Atributo | Tipo | Clave |
|---|---|---|
| mantencion_num | NUMBER(4) | PK / FK → MANTENCION |
| sucursal_id | CHAR(3) | PK / FK → MANTENCION |
| cod_servicio | NUMBER(3) | PK / FK → SERVICIO |
| descuento_serv | NUMBER(4,1) | Opcional |
| cantidad | NUMBER(3) | Obligatorio |

## Relaciones

- **PAIS (1,1) — CIUDAD (0,N):** un país puede tener cero o muchas ciudades; toda ciudad pertenece exactamente a un país.
- **CIUDAD (1,1) — SUCURSAL (0,N):** una ciudad puede tener cero o muchas sucursales; toda sucursal está ubicada en exactamente una ciudad.
- **MARCA (1,1) — MODELO (0,N):** una marca puede tener cero o muchos modelos; todo modelo pertenece a exactamente una marca. Es una relación identificadora, ya que `marca_id` forma parte de la PK de MODELO.
- **MODELO (1,1) — AUTOMOVIL (0,N):** un modelo puede estar asociado a cero o muchos automóviles (FK compuesta `cod_modelo`, `cod_marca`).
- **TIPO_AUTOMOVIL (1,1) — AUTOMOVIL (0,N):** un tipo puede clasificar a cero o muchos automóviles; todo automóvil tiene exactamente un tipo.
- **CLIENTE (1,1) — AUTOMOVIL (0,N):** un cliente puede tener cero o muchos automóviles; todo automóvil pertenece exactamente a un cliente.
- **CLIENTE (1,1) — ESTANDAR / PREMIUM (0,1):** jerarquía de subtipos; cada cliente se clasifica como estándar o premium, y cada subtipo comparte la PK del cliente.
- **SUCURSAL (1,1) — MANTENCION (0,N):** una sucursal puede registrar cero o muchas mantenciones; toda mantención se realiza en exactamente una sucursal. Tras el Caso 2 es una relación identificadora, ya que `cod_sucursal` forma parte de la PK de MANTENCION.
- **MECANICO (1,1) — MANTENCION (0,N):** un mecánico puede atender cero o muchas mantenciones; toda mantención es atendida por exactamente un mecánico.
- **AUTOMOVIL (0,1) — MANTENCION (0,N):** un automóvil puede tener cero o muchas mantenciones.
- **MECANICO (0,1) — MECANICO (0,N):** relación recursiva; un mecánico puede supervisar a cero o muchos mecánicos, y un mecánico puede tener cero o un supervisor.
- **MANTENCION (0,N) — SERVICIO (0,N):** relación N:M resuelta mediante la entidad asociativa DETALLE_SERVICIO, que además registra la cantidad y el descuento aplicado.

## Caso 2: Reglas de negocio agregadas con ALTER TABLE

| Restricción | Tipo | Regla |
|---|---|---|
| — | DROP COLUMN | Se elimina el atributo derivado `costo_total` de MANTENCION (se calcula a partir de los servicios). |
| MANTENCION_PK | PRIMARY KEY | La mantención se identifica por `num_mantencion` + `cod_sucursal`. |
| DETALLE_SERVICIO_PK | PRIMARY KEY | Se agrega `sucursal_id` y la PK pasa a ser (`mantencion_num`, `sucursal_id`, `cod_servicio`). |
| DET_SERV_FK_MANTENCION | FOREIGN KEY | FK compuesta hacia la nueva PK de MANTENCION. |
| CLIENTE_UN_EMAIL | UNIQUE | El email es opcional, pero no se puede repetir. |
| CLIENTE_CK_DV | CHECK | El dígito verificador debe ser 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 o 'K'. |
| MECANICO_CK_SUELDO | CHECK | El sueldo mínimo de un mecánico es de $510.000. |
| MANTENCION_CK_ESTADO | CHECK | El estado debe ser Reserva, Ingresado, Entregado o Anulado. |

## Caso 3: Poblamiento

El script respeta el orden de dependencia (de las tablas fuertes a las más débiles):

1. **PAIS** — IDENTITY genera los identificadores 9 (Chile), 12 (Perú) y 15 (Colombia).
2. **CIUDAD** — SEQ_CIUDAD genera 165 (Santiago), 170 (Lima) y 175 (Bogotá).
3. **SUCURSAL** — S01 (Providencia), S02 (Las 4 esquinas) y S03 (El Cafetero).
4. **SERVICIO** — SEQ_SERVICIO genera 400, 402, 404 y 406.
5. **MECANICO** — IDENTITY genera los identificadores del 460 al 523 (10 mecánicos); los supervisores son 460 y 474.
6. **MANTENCION** — mantenciones 101 a 105 distribuidas en las tres sucursales.

## Caso 4: Informes

### Informe 1 — Simulación de rebaja de impuestos

Lista a los mecánicos **sin bono de jefatura** (`bono_jefatura IS NULL`) y con impuestos menores a $40.000, mostrando su identificador, nombre, salario, impuesto actual, impuesto rebajado en un 20% `ROUND(monto_impuestos * 0.8)` y el sueldo con la rebaja aplicada. Se ordena por impuesto actual descendente y, en caso de empate, por apellido paterno ascendente.

### Informe 2 — Simulación de reajuste de sueldo

Lista a los mecánicos con sueldo superior a $600.000, mostrando su nombre completo, salario actual, el ajuste del 5% `ROUND(sueldo * 0.05)` y el sueldo reajustado. Se ordena por sueldo ascendente y luego por primer nombre descendente.

## Consideraciones de diseño

- Todas las restricciones tienen un nombre representativo según la tabla y su tipo (`_PK`, `_FK_`, `_UN_`, `_CK_`).
- `patente_auto` en MANTENCION se dejó opcional, ya que una mantención puede registrarse como **Reserva** antes de que el vehículo ingrese al taller.
- El cambio de clave primaria del Caso 2 obliga a eliminar primero la FK de DETALLE_SERVICIO y su PK, para luego recrearlas incluyendo la columna `sucursal_id`.
- El script comienza con un bloque PL/SQL que elimina las tablas y secuencias si ya existen, lo que permite ejecutarlo varias veces sin errores.
