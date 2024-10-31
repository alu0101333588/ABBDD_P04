# ABBDD_P04
Práctica 04 - Administración de Bases de Datos



## Restauración de la Base de Datos
Para la restauración de la base de datos alquilerdvd.tar se llevan a cabo los siguientes pasos:
- La base de datos se va a denominar **Alquilerdvd** mediante el comando:
  - `createdb Alquilerdvd`
- Se usa el usuario por defecto de PostgreSQL: `usuario` para restaurar la base de datos:
  - `pg_restore -d alquilerdvd -U usuario -h localhost -p 5432 /home/usuario/AlquilerPractica.tar`.
- Para verificar que la restauración se ha realizado con éxito, se accede a la base de datos a través del siguiente comando: `sudo -u postgres psql -d Alquierdvd`. Se puede verificar a través de la siguiente captura:


PONER IMAGEN AQUÍ


## Identificación de tablas, vistas y secuencias
- Para la identificación de las tablas se emplea: `\dt`.
- Para la identificación de las vistas: `\dv`.
- Para la identificación de las secuencias: `ds`.
- Se puede apreciar que existen 15 tablas y ninguna vista ni secuencias. 

## Identificación de tablas principales y sus elementos
- Como ya se ha comentado existen 15 tablas de las cuales se descatan:

### Customer
Se muestran los datos de los clientes:
- `customer_id`: Identificador del cliente
- `store_id`: Identificador de la tienda donde el cliente se dió de alta, se puede consultar en la tabla **Store**
- `first_name`: Nombre
- `last_name`: Apellido
- `email`: Correo electrónico 
- `address_id`: Identificador de los datos de dirección que se podrán consultar en la tabla **Address**
- `activebool`: Muestra si la cuenta del cliente está activa (`t` de true) o desactivada (`f` de false)
- `create_date`: Muestra la fecha en la que el cliente se dio de alta
- `last_update`: Fecha en la que se realizaron por última vez modificaciones en los datos del cliente
- `active`: Muestra, al igual que `activebool`si un cliente tiene la cuenta activa (1) o inactiva (0)

PONER IMAGEN AQUÍ

### Film
Se muestran los datos de las películas:
- `film_id`: Identificador de la película
- `title`: Título
- `description`: Descripción
- `release_year`: Año de lanzamiento
- `language_id: Identificador del lenguaje en el que está la película, que se puede consultar en la tabla **Language**.
- `rental_duration`: Duración en la que se puede alquilar la película, en días.
- `rental_rate`: Precio del alquiler
- `length`: Duración de la película, en minutos.
- `replacement_cost`: Sanción que se le pone al cliente, en caso de pérdidas o daños.
- `rating`: Indica a que público va dirigida la película.
- `last_update`: Fecha en la que se realizaron modificaciones por última vez.
- `special_features`: Características especiales, es decir, si cuenta con material adicional (tráilers, escenas postcréditos,...).
- `fulltext`: Listado de palabras clave

PONER IMAGEN AQUÍ

### Rental
Se muestra el listado de películas alquiladas por los clientes:
- `rental_id`: Identificador del alquiler de la película
- `rental_date`: Fecha en la que se alquila
- `inventory_id`: Identificador de la película, que se puede consultar en la tabla **Inventory**, permite identificar el número de copia de la misma.
- `customer_id`: Identificador del cliente que la alquiló, se puede consultar en la tabla **Customer**.
- `return_date`: Fecha de devolución
- `staff_id`: Identificador del empleado que realizó la operación de alquiler, se puede consultar en la tabla **Staff**.
- `last_update`: Fecha en la que se realizó la última modificación


PONER IMAGEN AQUÍ


### Staff
Se muestra el listado de los empleados:
- `staff_id`: Identificador del empleado
- `first_name`: Nombre
- `last_name`: Apellido
- `address_id`: Identificador de los datos de dirección que se podrán consultar en la tabla **Address**
- `email`: Correo electrónico
- `store_id`: Identificador de la tienda donde trabaja, se puede consultar en la tabla **Store**
- `active`: Muestra si el empleado está activo (t) o inactivo (f)
- `username`: Usuario del empleado
- `password`: Contraseña
- `last_update`: Fecha en la que se realizaron por última vez modificaciones en los datos del empleado
- `picture`: Foto del empleado

### Payment




  - Customer: Indica los datos de los clientes
  - Film: Alberga un listado de las películas
  - Rental: Indica que películas se han alquilado, junto a las fechas 
  - Staff:
  - Payment:

