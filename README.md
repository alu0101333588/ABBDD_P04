# ABBDD_P04
Práctica 04 - Administración de Bases de Datos



## Restauración de la Base de Datos
Para la restauración de la base de datos alquilerdvd.tar se llevan a cabo los siguientes pasos:
- La base de datos se va a denominar **Alquilerdvd** mediante el comando:
  - `createdb Alquilerdvd`
- Se usa el usuario por defecto de PostgreSQL: `usuario` para restaurar la base de datos:
  - `pg_restore -d alquilerdvd -U usuario -h localhost -p 5432 /home/usuario/AlquilerPractica.tar`.
- Para verificar que la restauración se ha realizado con éxito, se accede a la base de datos a través del siguiente comando: `sudo -u postgres psql -d Alquierdvd`. Se puede verificar a través de la siguiente captura:


PONER IMAGEN


## Identificación de tablas, vistas y secuencias
- Para la identificación de las tablas se emplea: `\dt`.
- Para la identificación de las vistas: `\dv`.
- Para la identificación de las secuencias: `ds`.
- Se puede apreciar que existen 15 tablas y ninguna vista ni secuencias. 

## Identificación de tablas principales y sus elementos
- Como ya se ha comentado existen 15 tablas de las cuales se descatan:

### Customer
Se muestran los datos de los clientes



  - Customer: Indica los datos de los clientes
  - Film: Alberga un listado de las películas
  - Rental: Indica que películas se han alquilado, junto a las fechas 
  - Staff:
  - Payment:

