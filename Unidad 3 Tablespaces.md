##Unidad 3 Tablespaces (undo,temporary and query's)

Ejemplo para crear un tablespace undo;

>create tablespace undi_prueba datafile '/u01/oradata/undo_prueba.dbf' size 40m;

Cosultar todos los tablesspace de la base de datos y que datafiles contiene cada uno de ellos:

> desc dba_data_files;
> select tablespace_name, file_name from dba_data_files;

Como estan administrados nuestros tablespaces:

> select tablespace_name,EXTENT_MANAGEMENT from dba_tablespaces;

Alterar el default temporal tablespace de la base de datos:

> alter database default temporary tablespace default_temp2;

Para encontrar el temporary tablespace puede hacer una consulta en:

> select * from database_properties;

Para borrar el temporary tablespace ejemplo:

>drop tablespacen temp;

####	Tablespace READ-ONLY and a READ WRITE

>aler tablespace userdata READ ONLY;

Para dar permisos de lectura y escritura:

>alter tablespace userdata READ WRITE;

1. Causa un checkpoint
2. Los datos solo son accesados para operaciones de lectura
3. Si podemos borrar una tabla pero no podemos hacer un rollback

####Tablespaces ONLINE and OFFLINE;
1. No se puede accesar los datos
2. Tablespaces que no se pueden ponerse fuera de línea;
     a) Tablespace SYSTEM
     b) Tablespace con segmentos activos de undo
     c) Tablespace con segmentos activos de undo
> alter tablespace userdata OFFLINE;
> alter tablespace userdata ONLINE;

######EJERCICIO 1:#######

Crear un usuario y asignarles el default tablespace y el temporal;
> create user prueba identified by prueba default tablespace  users temporblespace temp_example;
>grant create session to prueba
>grant create table to prueba

Crear una tabla y poner el tablespace de esa tabla en READ ONLY y al tratar de insertar nos mostrara lo siguiente:

>SQL> insert into uno values (2);
insert into uno values (2)
            *
ERROR at line 1:
ORA-01647: tablespace 'USERS' is read-only, cannot allocate space in it


```
Note: Dar permisos al usuario creado de quota sobre el tablespace que se le asigno para poder realizar el insert en una tabla:
```

>alter user prueba quota unlimited on tablespace;


Despues volver a dar los permisos al tablespace READ ONLY para que podamos insertar en la tabla creada con:

> alter tablespace tablespace_name READ WRITE;

Ponemos ahora en OFFLINE el tablespace y volvemos a insertar en la tabla de ese tablespace y nos aparecera lo siguiente:

> SQL> insert into uno values (2);
> ERROR at line 1:
> ORA-01542: tablespace 'USERS' is offline, cannot alocate space in it

Solo queda vover a alterar el tablespace y ponerlo de manera ONLINE.




######EJERCICIO 2:#######
Crear un tablespace de 20m maximo dentro de ese tablespace crear una tabla y generear un script que este insertando en ese tablespace, monitorear como va llenandose el tablespace;








