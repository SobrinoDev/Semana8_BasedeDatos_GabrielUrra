# Semana 8 – Base de Datos: Taller Mecánico Mikes Ltda.

Actividad sumativa individual **"Construyendo una base de datos a partir de un modelo relacional normalizado con sentencias SQL"**.

**Autor:** Gabriel Urra

## Contexto

El taller **Mikes Ltda.**, con sucursales en distintas ciudades del norte de Chile, ofrece servicios mecánicos multimarca (cambio de aceite, frenos, diagnóstico de motor, desabolladura, pintura, mantenimiento preventivo, entre otros). Cada mantención es atendida por un mecánico y se desglosa en distintos servicios. Los clientes se clasifican en **estándar** y **premium**.

A partir del modelo relacional normalizado entregado en la primera fase del proyecto, se implementa la base de datos en Oracle: creación del esquema, modificaciones por reglas de negocio, poblamiento y consultas.

## Contenido del repositorio

| Archivo / carpeta | Descripción |
|---|---|
| `Semana8_BasedeDatos_GabrielUrra.dmd` | Proyecto de **Oracle SQL Developer Data Modeler** (abrir este archivo). |
| `Semana8_BasedeDatos_GabrielUrra/` | Carpeta de metadatos del modelo (requerida por el `.dmd`, no modificar a mano). |
| `Semana8_BasedeDatos_GabrielUrra.sql` | Script completo con los casos 1 a 4. |

## Modelo de datos (Oracle Data Modeler)

El modelo incluye las siguientes tablas:

- **PAIS**, **CIUDAD**, **SUCURSAL** – ubicación de las sucursales.
- **SERVICIO** – servicios ofrecidos y su costo.
- **MARCA**, **MODELO**, **TIPO_AUTOMOVIL**, **AUTOMOVIL** – vehículos de los clientes.
- **CLIENTE** con sus subtipos **ESTANDAR** (puntaje de fidelidad) y **PREMIUM** (crédito).
- **MECANICO** – con relación recursiva de supervisor.
- **MANTENCION** – orden de mantención por sucursal.
- **DETALLE_SERVICIO** – servicios aplicados en cada mantención.

### Cómo abrir el modelo

1. Abrir **Oracle SQL Developer Data Modeler** (o SQL Developer → *Ver → Data Modeler*).
2. *Archivo → Abrir* y seleccionar `Semana8_BasedeDatos_GabrielUrra.dmd`.
3. Mantener la carpeta `Semana8_BasedeDatos_GabrielUrra/` junto al `.dmd`.

## Script SQL

### Caso 1 – Implementación del modelo
Creación de tablas desde las fuertes a las débiles, con restricciones PK, FK, UN y CK con nombres representativos.
- `PAIS.id_pais`: IDENTITY que inicia en 9 e incrementa en 3.
- `MECANICO.cod_mecanico`: IDENTITY que inicia en 460 e incrementa en 7.

### Caso 2 – Modificación del modelo (`ALTER TABLE`)
- Eliminación de la columna derivada `costo_total` en `MANTENCION`.
- PK de `MANTENCION` pasa a ser (`num_mantencion`, `cod_sucursal`) y se ajusta la FK en `DETALLE_SERVICIO`.
- `UNIQUE` sobre `CLIENTE.email` (opcional pero único).
- `CHECK` del dígito verificador: `0-9` o `K`.
- `CHECK` de sueldo mínimo del mecánico: $510.000.
- `CHECK` de estados de mantención: `Reserva`, `Ingresado`, `Entregado`, `Anulado`.

### Caso 3 – Poblamiento
- Secuencia `SEQ_SERVICIO` (inicia en 400, incrementa en 2).
- Secuencia `SEQ_CIUDAD` (inicia en 165, incrementa en 5).
- Inserción de datos en PAIS, CIUDAD, SUCURSAL, SERVICIO, MECANICO y MANTENCION.

### Caso 4 – Recuperación de datos
- Simulación de rebaja de impuestos (20%) para mecánicos sin bono de jefatura e impuestos menores a $40.000.
- Simulación de reajuste del 5% para mecánicos con sueldo superior a $600.000.

## Ejecución

1. Conectado como `SYS`/`SYSTEM` (o `ADMIN` en Oracle Cloud), ejecutar `PRY2204_Exp3_S8_Script_crea_usuario.sql` para crear el usuario `PRY2204_S8`.
2. Crear la conexión `PRY2204_SEMANA8` con ese usuario.
3. Ejecutar `Semana8_BasedeDatos_GabrielUrra.sql` como script (F5). El script puede re-ejecutarse: elimina previamente tablas y secuencias existentes.
