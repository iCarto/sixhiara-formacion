# Precauciones antes de manipular la base de datos

Cuando vayamos a realizar operaciones que puedan alterar el contenido de la base de datos: `DELETE`, `INSERT`, `UPDATE`, `ALTER`, ... es recomendable hacer primero una copia de seguridad.

En general siempre que se tengan dudas de la operación a realizar se debe realizar una copia de seguridad. Las sentencias `SELECT` cuando se llama a una función también podrían alterar el contenido por ejemplo si alteramos una secuencia `SELECT setval('utentes.utentes_gid_seq', 5000, false);`

Hay dos formas de hacer una copia:

* Hacer un dump de la base de datos al ordenador de modo que podamos restaurarla si hay algún problema. Podemos seguir estas instrucciones:  https://gitlab.com/icarto/ikdb/-/blob/master/manuales/postgres/dump/postgresql_dump_pgadmin.md

* Crear una nueva base de datos a partir de una existente

## Crear una nueva base de datos a partir de una existente

La sentencia `CREATE DATABASE` admite usar una base de datos como `template`. Esto permite crear un clon o copia de la base de datos, en el propio servidor (cluster) de PostgreSQL.

Estas bases de datos temporales deben eliminarse pasado un tiempo prudencial.

Las instrucciones para hacer esto de forma segura serían.

### Conectar a la base de datos `postgres`

Conectar a la base de datos `postgres`. No se puede usar una base de datos como `template` si está siendo usada por algún cliente. En psql (el cliente de consola de PostgreSQL se puede hacer mediante el comando

```sql
\c postgres
```

En pgAdmin:

* Clicar encima de la base de datos "postgres"
* Desconectar del resto de bases, (click derecho sobre ellas y `Disconnect`
* Cerrar las ventanas de "Query Tool" abiertas
* Abrir una nueva ventana de "Query Tool" asegurandonos que la conexión es sobre "postgres"

### Expulsar a los usuarios

Como explicamos, no se puede usar una base de datos como `template` si está siendo usada por algún cliente (seamos nosotros u otro usuario)

Primero listamos los clientes conectados a la base de datos `test`:

```sql
SELECT datname, usename, application_name, client_addr FROM pg_stat_activity WHERE datname = 'test';
```

Luego los expulsamos. Esta operación interrumpe las operaciones que estén realizando en ese momento, por lo que deberíamos avisar primero

```sql
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname='test';
```

### Crear la copia de la base de datos 

La siguiente sentencia crea una base de datos llamada `test_bck_211110` usando como template la base de datos `test` y prohibe a cualquier usuario conectarse a la nueva base de datos para evitar que se use o modifique accidentalmente.

Es importante que usemos siempre la misma nomenclatura para las copias de la base de datos para evitar problemas. Por ejemplo el nombre de la base de datos, más un sufijo como `bck` más la fecha en formato `yymmdd` 

```sql
CREATE DATABASE test_bck_211110 WITH TEMPLATE test ALLOW_CONNECTIONS false;
```

### Conectar de nuevo a la base de datos de trabajo

Una vez hemos creado la copia podemos acceder de nuevo a la base de datos de trabajo. Si usamos pgAdmin cerramos todas las ventanas de "Query Tool", pulsamos sobre la de trabajo, `test` en este ejemplo y abrimos un nuevo "Query Tool".

En `psql` simplemente teclearemos `\c test`
