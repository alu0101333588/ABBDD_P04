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
- Para verificar que la restauración se ha realizado con éxito, se accede a la base de datos a través del siguiente comando: `sudo -u postgres psql -d Alquilerdvd`. Se puede verificar a través de la siguiente captura:


![Restauracion](/images/restauracion_basededatos.png "Restauracion")


## Identificación de tablas, vistas y secuencias
- Para la identificación de las tablas se emplea: `\dt`.
- Para la identificación de las vistas: `\dv`.
- Para la identificación de las secuencias: `\ds`.
- Se puede apreciar que existen 15 tablas, ninguna vista y 13 secuencias. 

![Tablas](/images/tablas_bbdd.png "Tablas")
![Secuencias](/images/secuencias_bbdd.png "Secuencias")

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

![Customer](/images/customer_table.png "Customer")

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

![Film](/images/film_table.png "Film")

### Rental
Se muestra el listado de películas alquiladas por los clientes:
- `rental_id`: Identificador del alquiler de la película
- `rental_date`: Fecha en la que se alquila
- `inventory_id`: Identificador de la película, que se puede consultar en la tabla **Inventory**, permite identificar el número de copia de la misma.
- `customer_id`: Identificador del cliente que la alquiló, se puede consultar en la tabla **Customer**.
- `return_date`: Fecha de devolución
- `staff_id`: Identificador del empleado que realizó la operación de alquiler, se puede consultar en la tabla **Staff**.
- `last_update`: Fecha en la que se realizó la última modificación


![Rental](/images/rental_table.png "Rental")


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

![Staff](/images/staff_table.png "Staff")

### Payment
Se muestran los pagos realizados
- `payment_id`: Identificador de la transación
- `customer_id`: Identificador del cliente que ha realizado el pago, se puede consultar en la tabla **Customer**
- `staff_id`: Identificador del empleado que realizó la operación de alquiler, se puede consultar en la tabla **Staff**.
- `amount`: Cantidad de dinero que ha pagado
- `payment_date`: Fecha en la que realizó el pago.

![Payment](/images/payment_table.png "Payment")

## Consultas
### Ventas totales por categoría de películas ordenadas descendentemente.
- Se requiere emplear las siguientes tablas: `payment`, `rental`, `inventory`, `film_category`, `category`
- Se requiere emplear los siguientes atributos: `amount`, `rental_id`, `inventory_id`, `film_id`, `category_id`, `name`

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
![ventas_por_categoria](/images/ventas_por_categoria.png)

Se selecciona el atributo `amount`, empleando `SUM` para realizar un sumatorio de todos los precios de las películas, que si están presentes en la tabla payment significa que algún cliente ha pagado ese dinero por esa película que pertenece a una categoría. Además, se selecciona el nombre de la categoría (`name`). Para ambos atributos se emplean las etiquetas "Ventas" y "Categoría", con el objetivo de mostrar de manera más clara la tabla final.
Se emplea `JOIN` para fusionar los atributos comunes de las tablas necesarias para la consulta. El `GROUP BY` es necesario para agrupar por el nombre de categorías y así unificar las categorías para obtener las ventas totales. Finalmente, se ordena descendentemente empleando con `ORDER BY` y `DESC`.


### Ventas totales por tienda, donde se refleje la ciudad, el país (concatenar la ciudad y el país empleando como separador la “,”), y el encargado. Pudiera emplear GROUP BY, ORDER BY
- Se requiere emplear las siguientes tablas: `payment`, `staff`, `store`, `address`, `city`, `country`
- Se requiere emplear los siguientes atributos: `amount`, `staff_id`, `store_id`, `address_id`, `city_id`, `country_id`, `country`, `city`. `staff_id`, `first_name`, `last_name`

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

![ventas_por_tienda](/images/ventas_por_tienda.png)

Se selecciona el atributo `amount`, empleando `SUM` para realizar un sumatorio de todos los precios de las películas, que si están presentes en la tabla payment significa que algún cliente ha pagado ese dinero por esa película que pertenece a una categoría. Además, se selecciona los atributos requeridos en el enunciado (`store_id`, `address_id`, `city_id`, `country_id`, `staff_id`, `first_name`, `last_name`). En el caso del nombre de la ciudad y del país se emplea `CONCAT` para unir las columnas en una sola denominada `Ubicación`.
Se emplea `JOIN` para fusionar los atributos comunes de las tablas necesarias para la consulta. El `GROUP BY` es necesario para agrupar por los nombres de los atributos que se desean mostrar.



### Lista de películas, donde se reflejen el identificador, el título, descripción, categoría, el precio, la duración de la película, clasificación, nombre y apellidos de los actores (puede realizar una concatenación de ambos). Pudiera emplear GROUP BY
- Se requiere emplear las siguientes tablas: `film`, `category`, `film_category`, `film_actor`, `actor`
- Se requiere emplear los siguientes atributos: `film_id`, `title`, `description`, `category_id`, `name`, `rental_rate`, `length`, `rating`, `actor_id`, `first_name`, `last_name`

```
SELECT f.film_id AS Id_Pelicula, title AS Titulo, description AS Descripcion, name AS Categoria, rental_rate AS Precio, length AS Duración, rating AS Calificación, CONCAT(first_name, ', ', last_name) AS Actor
FROM film f
JOIN film_category fc ON fc.film_id = f.film_id
JOIN category c ON c.category_id = fc.category_id
JOIN film_actor fa ON fa.film_id = fc.film_id
JOIN actor a ON a.actor_id = fa.actor_id
GROUP BY f.film_id, title, description, name, rental_rate, length, rating, first_name, last_name
ORDER BY f.film_id;
```

![detalles_peliculas](/images/detalles_peliculas.png)


Se seleccionan los atributos requeridos en el enunciado (`film_id`, `title`, `description`, `category_id`, `name`, `rental_rate`, `length`, `rating`, `actor_id`, `first_name`, `last_name`). En el caso del nombre y apellidos del actor se emplea `CONCAT` para unir las columnas en una sola denominada `Actor`.
Se emplea `JOIN` para fusionar los atributos comunes de las tablas necesarias para la consulta. El `GROUP BY` es necesario para agrupar por los nombres de los atributos que se desean mostrar.

  
### Información de los actores, donde se incluya sus nombres y apellidos, las categorías y sus películas. Los actores deben de estar agrupados y, las categorías y las películas deben estar concatenados por “:” 
- Se requiere emplear las siguientes tablas: `actor`, `film_actor`, `category`, `film_category`, `film` 
- Se requieren emplear los siguientes atributos: `first_name`, `last_name`, `actor_id`, `film_id`, `category_id`. `name`, `title`.

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

![peliculas_por_actor](/images/peliculas_por_actor.png)


Se selecciona `nombre_actor` (`first_name` y `last_name`), se emplea `STRING_AGG` junto con `CONCAT` para crear una lista de categorías y sus películas, con cada par (`categoría`, lista de `peliculas`) separado por un salto de línea, se renombra como `categorias_peliculas`.
Se lleva a cabo una subconsulta para obtener, para cada actor y categoría, una lista de títulos de películas agrupados y concatenados. A través de `STRING_AGG(f.title, ', ' ORDER BY f.title)` se van a mostrar diferentes títulos de películas en una única fila, ordenados.
Finalmente, es necesario agrupar por `nombre_actor`, y de cara a una mejor visualización de los resultados se agrupa también por `nombre_actor`.


## Vistas
### Ventas totales por categoría de películas ordenadas descendentemente.

```
CREATE VIEW view_ventas_por_categoria AS (
  SELECT SUM(amount) AS Ventas, name AS Categoria
  FROM payment p
  JOIN rental r ON r.rental_id = p.rental_id
  JOIN inventory i ON i.inventory_id = r.inventory_id
  JOIN film_category f ON f.film_id = i.film_id
  JOIN category c ON c.category_id = f.category_id
  GROUP BY name
  ORDER BY Ventas DESC
);
```

![view_ventas_por_categoria](/images/view_ventas_por_categoria.png)

### Ventas totales por tienda, donde se refleje la ciudad, el país (concatenar la ciudad y el país empleando como separador la “,”), y el encargado. 

```
CREATE VIEW view_ventas_por_tienda AS (
  SELECT SUM(amount) AS Ventas, s.store_id AS Tienda, CONCAT(c.city, ', ', co.country) AS Ubicación, s.staff_id AS Id_Encargado, s.first_name AS     Nombre, s.last_name AS Apellido
  FROM payment p
  JOIN staff s ON s.staff_id = p.staff_id
  JOIN store st ON st.store_id = s.store_id
  JOIN address a ON a.address_id = st.address_id
  JOIN city c ON c.city_id = a.city_id
  JOIN country co ON co.country_id = c.country_id
  GROUP BY s.store_id, c.city, co.country, s.staff_id, s.first_name, s.last_name
);
```

![view_ventas_por_tienda](/images/view_ventas_por_tienda.png)


### Lista de películas, donde se reflejen el identificador, el título, descripción, categoría, el precio, la duración de la película, clasificación, nombre y apellidos de los actores (puede realizar una concatenación de ambos).

```
CREATE VIEW view_detalles_peliculas AS (
  SELECT f.film_id AS Id_Pelicula, title AS Titulo, description AS Descripcion, name AS Categoria, rental_rate AS Precio, length AS Duración, rating AS Calificación, CONCAT(first_name, ', ', last_name) AS Actor
  FROM film f
  JOIN film_category fc ON fc.film_id = f.film_id
  JOIN category c ON c.category_id = fc.category_id
  JOIN film_actor fa ON fa.film_id = fc.film_id
  JOIN actor a ON a.actor_id = fa.actor_id
  GROUP BY f.film_id, title, description, name, rental_rate, length, rating, first_name, last_name
  ORDER BY f.film_id
);
```

![view_detalles_peliculas](/images/view_detalles_peliculas.png)


### Información de los actores, donde se incluya sus nombres y apellidos, las categorías y sus películas. Los actores deben de estar agrupados y, las categorías y las películas deben estar concatenados por “:”

```
CREATE VIEW view_peliculas_por_actor AS (
  SELECT nombre_actor, STRING_AGG(CONCAT(categoria, ': ', peliculas), E'\n') AS categorias_peliculas
  FROM (
      SELECT CONCAT(a.first_name, ' ', a.last_name) AS nombre_actor, c.name AS categoria, STRING_AGG(f.title, ', ' ORDER BY f.title) AS peliculas
      FROM actor a
      JOIN film_actor fa ON a.actor_id = fa.actor_id
      JOIN film f ON fa.film_id = f.film_id
      JOIN film_category fc ON f.film_id = fc.film_id
      JOIN category c ON fc.category_id = c.category_id
      GROUP BY a.first_name, a.last_name, c.name
  )  AS subconsulta
  GROUP BY nombre_actor
  ORDER BY nombre_actor
);
```

![view_peliculas_por_actor](/images/view_peliculas_por_actor.png)


__________________________________
__________________________________








```
ALTER TABLE address
ADD CONSTRAINT codigo_postal_no_nulo CHECK (postal_code IS NOT NULL);
```

```
ALTER TABLE customer
ADD CONSTRAINT email_no_nulo CHECK (email IS NOT NULL);
```

## Sentencia last_updated

Si queremos ver qué contiene la función last_updated, ejecutamos la siguiente consulta;

```
SELECT proname AS last_updated,
       pg_get_functiondef(p.oid) AS function_definition
FROM pg_proc p
JOIN pg_namespace n ON p.pronamespace = n.oid
WHERE p.proname = 'last_updated';
```

`last_updated` también está presente en las siguientes tablas: Actor, Address, Category, City, Country, Customer, Film, Film_actor, Film_category, Inventory, Language, Rental, Staff, Store.
No está presente en Payment.
La función está en uso con un trigger, que lo que hace es actualizar la columna de última actualización cuando se produce una modificación de una fila de la tabla.

![last_updated](/images/function_last_update.png)


También hay otra función dentro de otro trigger:
```
SELECT proname AS tsvector_update_trigger,
       pg_get_functiondef(p.oid) AS function_definition
FROM pg_proc p
JOIN pg_namespace n ON p.pronamespace = n.oid
WHERE p.proname = 'tsvector_update_trigger';
```

![tsvector](/images/function_tsvector_update_trigger.png)

## Trigger film_insert

Creamos la tabla primero donde vamos a guardar la fecha de las inserciones nuevas que se realicen en la tabla Film:

```
CREATE TABLE film_insert_log (
    log_id SERIAL PRIMARY KEY,
    film_id INTEGER NOT NULL,
    insert_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```
Pasamos a crear la función que va a ser ejecutada dentro del trigger. Va a insertar en la tabla creada anteriormente el id de la película que se insertó y la fecha en la que se ha realizado:

```
CREATE OR REPLACE FUNCTION log_film_insert()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO film_insert_log (film_id, insert_date)
    VALUES (NEW.film_id, CURRENT_TIMESTAMP);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
Para finalizar, creamos el trigger que llamará a la función después de cada inserción en la tabla anterior:

```
CREATE TRIGGER film_insert_trigger
AFTER INSERT ON film
FOR EACH ROW
EXECUTE FUNCTION log_film_insert();
```
Vamos a probar el funcionamiento con un ejemplo:

```
INSERT INTO film (title, description, release_year, language_id, rental_duration, rental_rate, length, replacement_cost, rating, special_features, fulltext)
VALUES ('The Great Adventure', 'An epic journey across unknown lands.', 2023, 1, 5, 3.99, 120, 24.99, 'PG', ARRAY['Deleted Scenes', 'Behind the Scenes'], to_tsvector('english', 'The Great Adventure An epic journey across unknown lands.'));
```

![film_trigger](/images/film_insert_trigger.png)

## Trigger film_delete

Creamos la tabla para almacenar los datos de cuando se eliminó una película:

```
CREATE TABLE film_delete_log (
    log_id SERIAL PRIMARY KEY,
    film_id INTEGER NOT NULL,
    film_name VARCHAR(255),
    delete_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

Creamos una función que se ejecutará dentro del trigger. Va a insertar los valores antiguos de film_id y el título de la película, junto a la fecha en la que se eliminó:

```
CREATE OR REPLACE FUNCTION log_film_delete()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO film_delete_log (film_id, film_name, delete_date)
    VALUES (OLD.film_id, OLD.title, CURRENT_TIMESTAMP);
    RETURN OLD;
END;
$$ LANGUAGE plpgsql;
```

Creamos el trigger ahora, que se invocará después de una eliminación en la tabla Film:

```
CREATE TRIGGER film_delete_trigger
AFTER DELETE ON film
FOR EACH ROW
EXECUTE FUNCTION log_film_delete();
```

Probamos con un ejemplo:

![delete_trigger](/images/film_delete_trigger.png)


