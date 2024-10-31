# ABBDD_P04
Práctica 04 - Administración de Bases de Datos



## Restauración de la Base de Datos
Para la restauración de la base de datos alquilerdvd.tar se llevan a cabo los siguientes pasos:
- La base de datos se va a denominar **Alquilerdvd** mediante el comando:
  - `createdb Alquilerdvd`
- Se usa el usuario por defecto de PostgreSQL: `usuario` oara restaurar la base de datos:
  - pg_restore -d alquilerdvd -U usuario -h localhost -p 5432 /home/usuario/AlquilerPractica.tar
 - Para verificar que la restauración se ha realizado con éxito, se accede a la base de datos a través del siguiente comando: `sudo -u postgres psql -d Alquierdvd`. Se puede verificar a través de la siguiente captura:


PONER IMAGEN


## Identificación de tablas, vistas y secuencias
- Se accede a la base de datos a través del siguiente comando: `sudo -u postgres psql -d Alquierdvd`.
- 
