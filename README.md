# Prácticas de Oracle PL/SQL

Colección de ejercicios para aprender PL/SQL de forma progresiva: desde bloques anónimos y variables hasta procedimientos, funciones, cursores y manejo de excepciones. Cada carpeta aborda un tema y contiene ejemplos pequeños o soluciones a tareas; las consignas originales están disponibles en PDF.

El objetivo es experimentar con el lenguaje y sus herramientas, no ofrecer una aplicación lista para producción. Los valores de entrada suelen estar definidos dentro de los scripts para poder modificarlos y observar distintos resultados.

## Stack y herramientas

- **Oracle Database:** motor de base de datos relacional utilizado por los ejemplos.
- **SQL y PL/SQL:** consultas y modificaciones de datos, bloques anónimos y unidades almacenadas.
- **Cliente compatible con Oracle:** por ejemplo, SQL Developer, SQL*Plus o SQLcl. El cliente debe permitir ejecutar scripts y visualizar la salida de `DBMS_OUTPUT`.
- **PDF:** consignas de las tareas y material de referencia sobre excepciones predefinidas.

No se especifica una versión concreta de Oracle ni se incluyen una base de datos, un entorno de instalación o una suite automatizada de pruebas.

## Recorrido por el repositorio

| Carpeta | Contenido |
| --- | --- |
| [0. Consignas de las tareas](0.%20Consignas%20de%20las%20tareas/) | Seis archivos PDF con los enunciados. |
| [1. Empecemos a escribir PLs](1.%20Empecemos%20a%20escribir%20PLs/) | Bloques básicos, variables, constantes, `SELECT INTO`, funciones de Oracle, arreglos y cálculo de una compra. |
| [2. Condiciones y Operadores](2.%20Condiciones%20y%20Operadores/) | `IF`, `ELSIF`, `CASE`, operadores y cálculo de un aumento salarial por rangos. |
| [3. Cicloes en PL-SQL](3.%20Cicloes%20en%20PL-SQL/) | Ciclos `FOR`, `LOOP` y `WHILE`, `EXIT`, `CONTINUE`, promedio de notas y factorial. |
| [4. Procedimientos y Funciones](4.%20Procedimientos%20y%20Funciones/) | Datos de ejemplo, funciones, procedimientos, parámetros y cálculo de promedios. |
| [5. Cursores y Tipos de dato Record](5.%20Cursores%20y%20Tipos%20de%20dato%20Record/) | Cursores, atributos, registros y una práctica de liquidación de salarios. |
| [6. Excepciones](6.%20Excepciones/) | Captura de errores, excepciones personalizadas y material sobre excepciones predefinidas. |

La numeración refleja el recorrido temático; algunas carpetas tienen saltos en la numeración de sus ejercicios.

## Conceptos y funcionalidades practicadas

- **Fundamentos:** estructura `DECLARE` / `BEGIN` / `END`, tipos como `NUMBER`, `NVARCHAR2`, `DATE` y `BOOLEAN`, constantes, asignaciones y salida con `DBMS_OUTPUT`.
- **SQL dentro de PL/SQL:** consultas con `SELECT ... INTO`, uso de `DUAL`, agregaciones y actualización de registros.
- **Funciones de Oracle:** manipulación de texto y fechas con `TRIM`, `SUBSTR`, `REPLACE`, `SYSDATE`, `ADD_MONTHS` y `TO_CHAR`, entre otras; tratamiento de valores nulos con `NVL`.
- **Colecciones y control de flujo:** arreglos `VARRAY`, decisiones con `IF` y `CASE`, ciclos y control de su ejecución.
- **Funciones y procedimientos:** creación de unidades almacenadas y locales, parámetros `IN` y `OUT`, cálculo de edad, comparación de números, conteo de empleados y promedios de salarios y notas.
- **Cursores y registros:** recorrido de consultas con `FOR`, cursores explícitos con `OPEN`, `FETCH` y `CLOSE`, atributos como `SQL%ROWCOUNT` y `%NOTFOUND`, registros `%ROWTYPE` y `RECORD` definidos por el usuario.
- **Excepciones:** manejo con `EXCEPTION` y `WHEN OTHERS`, declaración y lanzamiento de excepciones propias con `RAISE`, y un ejemplo comentado de `NO_DATA_FOUND`.

Entre los ejercicios aplicados se encuentran el cálculo de subtotal, impuesto y total de una compra; clasificación y promedio de notas; aumentos salariales; factorial; y cálculo de salario diario y a pagar según días trabajados.

## Cómo ejecutar los ejercicios

### 1. Preparar el entorno

1. Contá con una instancia de Oracle Database accesible y una cuenta con permisos para conectarse. Para los ejercicios que crean tablas o unidades almacenadas, necesitás además los permisos correspondientes en tu esquema.
2. Cloná el repositorio y conectate a la base desde tu cliente Oracle.
3. Trabajá preferentemente en un **esquema de pruebas**: algunos scripts crean o reemplazan objetos, actualizan datos y ejecutan `COMMIT`.

### 2. Ejecutar un primer bloque

Abrí [Hola_mundo.sql](1.%20Empecemos%20a%20escribir%20PLs/1.%20Hola%20Mundo%20en%20PL-SQL/Hola_mundo.sql) y ejecutalo como **script completo**. En SQL Developer podés usar «Run Script» (F5); en SQL*Plus o SQLcl, desde la raíz del repositorio:

```sql
@"1. Empecemos a escribir PLs/1. Hola Mundo en PL-SQL/Hola_mundo.sql"
```

El script activa `SET SERVEROUTPUT ON` y debería mostrar `Hola Mundo` y `Hola Fernando`. Si tu cliente no interpreta ese comando, habilitá la visualización de `DBMS_OUTPUT` en su interfaz. La barra `/` al final de los bloques y definiciones indica al cliente que los ejecute; usá el modo de ejecución de scripts para procesarla junto con comandos como `SET`.

Los ejemplos básicos suelen ser autónomos. Podés cambiar las variables iniciales y volver a ejecutarlos para explorar otros resultados.

### 3. Preparar las tablas para los ejercicios con datos

Ejecutá estos scripts una vez en el esquema de pruebas, según lo que quieras practicar:

1. [Tabla y data.sql](4.%20Procedimientos%20y%20Funciones/0.%20Preparando%20el%20ambiente%20de%20trabajo/Tabla%20y%20data.sql) crea y carga `EMPLEADOS`; incluye un `COMMIT`.
2. [Tabla de Notas.sql](4.%20Procedimientos%20y%20Funciones/7.%20Tarea%20de%20Procedimientos%20y%20Funciones/Tabla%20de%20Notas.sql) crea y carga `NOTAS`. No incluye `COMMIT`: si necesitás usar esos datos desde otra sesión, confirmá la transacción primero.

Los scripts usan `CREATE TABLE`, por lo que volver a ejecutarlos en un esquema donde ya existan esas tablas dará un error. Revisá los objetos y datos existentes antes de crear, modificar o eliminar nada.

Por ejemplo, después de preparar `EMPLEADOS`, ejecutá [Funcion basica.sql](4.%20Procedimientos%20y%20Funciones/1.%20Funcion%20Basica/Funcion%20basica.sql) para crear `total_empleados` y luego consultala:

```sql
SELECT total_empleados() AS total FROM dual;
```

Con los datos originales de ejemplo, el resultado es **10**. De forma similar, los ejercicios de cursores y excepciones que consultan `EMPLEADOS` o `NOTAS` requieren que la tabla correspondiente exista y esté cargada.

## Consideraciones para practicar

- La [solución de la tarea de cursores](5.%20Cursores%20y%20Tipos%20de%20dato%20Record/9.%20Resolucion%20de%20la%20tarea/Solucion%20tarea%20de%20cursores.sql) utiliza `EMPLEADOS.DIASTRABAJADOS`, columna que **no está incluida** en el script de creación de `EMPLEADOS`. Hay que adaptar el esquema y cargar ese dato, o adaptar el ejercicio, antes de ejecutarlo. Además, su salida etiquetada como «Nombre» imprime el ID del empleado.
- El procedimiento `actualiza_fecha_empleados` actualiza todos los empleados; otros ejemplos también modifican datos o confirman transacciones. Leé cada script antes de ejecutarlo.
- Algunos ejemplos dependen de la configuración de sesión, por ejemplo el formato de fecha implícito, o generan valores aleatorios con `DBMS_RANDOM`; la salida puede variar.
- Son ejercicios didácticos: algunos simplifican la validación y el manejo de errores. Revisalos y adaptalos antes de reutilizarlos fuera de un entorno de aprendizaje.
