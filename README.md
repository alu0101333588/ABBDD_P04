# ABBDD_P04
Práctica 04 - Administración de Bases de Datos
- Realizada por Andrés Hernández Ortega y Luka Kravarusic Sljapic



## Restauración de la Base de Datos
Para la restauración de la base de datos alquilerdvd.tar se llevan a cabo los siguientes pasos:
- Para acceder previamente: `sudo -u postgres psql`.
- La base de datos se va a denominar **Alquilerdvd** mediante el comando:
  - `createdb Alquilerdvd`.
- Se usa el usuario por defecto de PostgreSQL: `usuario` para restaurar la base de datos:
  - `pg_restore -d alquilerdvd -U usuario -h localhost -p 5432 /home/usuario/AlquilerPractica.tar`.
- Para verificar que la restauración se ha realizado con éxito, se accede a la base de datos a través del siguiente comando: `sudo -u postgres psql -d Alquierdvd`. Se puede verificar a través de la siguiente captura:


PONER IMAGEN AQUÍ


## Identificación de tablas, vistas y secuencias
- Para la identificación de las tablas se emplea: `\dt`.
- Para la identificación de las vistas: `\dv`.
- Para la identificación de las secuencias: `ds`.
- Se puede apreciar que existen 15 tablas y ninguna vista ni secuencias. 

PONER IMAGEN AQUÍ

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
Se muestran los pagos realizados
- `payment_id`: Identificador de la transación
- `customer_id`: Identificador del cliente que ha realizado el pago, se puede consultar en la tabla **Customer**
- `staff_id`: Identificador del empleado que realizó la operación de alquiler, se puede consultar en la tabla **Staff**.
- `amount`: Cantidad de dinero que ha pagado
- `payment_date`: Fecha en la que realizó el pago.



## Consultas
### Obtenga las ventas totales por categoría de películas ordenadas descendentemente.
- Se requiere emplear las siguientes tablas: `payment`, `rental`, `inventory`, `film_category`, `category`
- Se requieren emplear los siguientes atributos: `amount`, `rental_id`, `inventory_id`, `film_id`, `category_id`, `name`

```
SELECT SUM(amount) AS Ventas, name AS Categoria
FROM payment p
JOIN rental r ON r.rental_id = p.rental_id
JOIN inventory i ON i.inventory_id = r.inventory_id
JOIN film_category f ON f.film_id = i.film_id
JOIN category c ON c.category_id = f.category_id
GROUP BY name
ORDER BY Ventas DESC;
```
PONER FOTO AQUÍ

Se selecciona el atributo `amount`, empleando `SUM` para realizar un sumatorio de todos los precios de las películas, que si están presentes en la tabla payment significa que algún cliente ha pagado ese dinero por esa película que pertenece a una categoría. Además, se selecciona el nombre de la categoría (`name`). Para ambos atributos se emplean las etiquetas "Ventas" y "Categoría", con el objetivo de mostrar de manera más clara la tabla final.
Se emplea `JOIN` para fusionar los atributos comunes de las tablas necesarias para la consulta. El `GROUP BY` es necesario para agrupar por el nombre de categorías y así unificar las categorías para obtener las ventas totales. Finalmente, se ordena descendentemente empleando con `ORDER BY` y `DESC`.


### Obtenga las ventas totales por tienda, donde se refleje la ciudad, el país (concatenar la ciudad y el país empleando como separador la “,”), y el encargado. Pudiera emplear GROUP BY, ORDER BY
- Se requiere emplear las siguientes tablas: `payment`, `staff`, `store`, `address`, `city`, `country`
- Se requieren emplear los siguientes atributos: `amount`, `staff_id`, `store_id`, `address_id`, `city_id`, `country_id`, `country`, `city`. `staff_id`, `first_name`, `last_name`

```
SELECT SUM(amount) AS Ventas, s.store_id AS Tienda, CONCAT(c.city, ', ', co.country) AS Ubicación, s.staff_id AS Id_Encargado, s.first_name AS Nombre, s.last_name AS Apellido
FROM payment p
JOIN staff s ON s.staff_id = p.staff_id
JOIN store st ON st.store_id = s.store_id
JOIN address a ON a.address_id = st.address_id
JOIN city c ON c.city_id = a.city_id
JOIN country co ON co.country_id = c.country_id
GROUP BY s.store_id, c.city, co.country, s.staff_id, s.first_name, s.last_name;
```

PONER FOTO AQUÍ

Se selecciona el atributo `amount`, empleando `SUM` para realizar un sumatorio de todos los precios de las películas, que si están presentes en la tabla payment significa que algún cliente ha pagado ese dinero por esa película que pertenece a una categoría. Además, se selecciona los atributos requeridos en el enunciado (`store_id`, `address_id`, `city_id`, `country_id`, `staff_id`, `first_name`, `last_name`). En el caso del nombre de la ciudad y del país se emplea `CONCAT` para unir las columnas en una sola denominada `Ubicación`.
Se emplea `JOIN` para fusionar los atributos comunes de las tablas necesarias para la consulta. El `GROUP BY` es necesario para agrupar por los nombres de los atributos que se desean mostrar.


### Obtenga una lista de películas, donde se reflejen el identificador, el título, descripción, categoría, el precio, la duración de la película, clasificación, nombre y apellidos de los actores (puede realizar una concatenación de ambos). Pudiera emplear GROUP BY
- Se requiere emplear las siguientes tablas: `film`, `category`, `film_category`, `film_actor`, `actor`
- Se requieren emplear los siguientes atributos: `film_id`, `title`, `description`, `category_id`, `name`, `rental_rate`, `length`, `rating`, `actor_id`, `first_name`, `last_name`

```
SELECT nombre_actor, STRING_AGG(CONCAT(categoria, ': ', peliculas), E'\n') AS categorias_peliculas
FROM (
    SELECT CONCAT(a.first_name, ' ', a.last_name) AS nombre_actor, c.name AS categoria, STRING_AGG(f.title, ', ' ORDER BY f.title) AS peliculas
    FROM actor a
    JOIN film_actor fa ON a.actor_id = fa.actor_id
    JOIN film f ON fa.film_id = f.film_id
    JOIN film_category fc ON f.film_id = fc.film_id
    JOIN category c ON fc.category_id = c.category_id
    GROUP BY a.first_name, a.last_name, c.name
) AS subconsulta
GROUP BY nombre_actor
ORDER BY nombre_actor;
```

Se selecciona `nombre_actor` (`first_name` y `last_name`), se emplea `STRING_AGG` junto con `CONCAT` para crear una lista de categorías y sus películas, con cada par (`categoría`, lista de `peliculas`) separado por un salto de línea, se renombra como `categorias_peliculas`.
Se lleva a cabo una subconsulta para obtener, para cada actor y categoría, una lista de títulos de películas agrupados y concatenados. A través de `STRING_AGG(f.title, ', ' ORDER BY f.title)` se van a mostrar diferentes títulos de películas en una única fila, ordenados.
Finalmente, es necesario agrupar por `nombre_actor`, y de cara a una mejor visualización de los resultados se agrupa también por `nombre_actor`.












