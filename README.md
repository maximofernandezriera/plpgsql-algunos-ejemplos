# plpgsql-algunos-ejemplos

## Un ejemplo con un select y %type en esta porción de código:

      create or replace function find_film_by_id(p_film_id int)
      returns varchar
      language plpgsql
      as $$
      declare
         film_title film.title%type;
      begin
        -- find film title by id
        select title 
        into film_title
        from film
        where film_id = p_film_id;

        if not found then
           raise 'Film with id % not found', p_film_id;
        end if;

        return title;

      end;$$

Vemos que definimos una función llamada find_film_by_id que toma un parámetro p_film_id de tipo int y devuelve un valor de tipo varchar.

La función declara una variable local llamada film_title del mismo tipo que la columna title de la tabla film. Luego, la función ejecuta una consulta para seleccionar el título de la película con el film_id igual al valor del parámetro p_film_id y lo guarda en la variable film_title.

Si no se encuentra ninguna película con ese film_id, la función genera un error con el mensaje 'Film with id % not found', donde % se reemplaza por el valor del parámetro p_film_id.

Finalmente, la función devuelve el valor de la variable film_title.

# Para probar find_film_by_id

Deberíamos crear la base de datos y la tabla film, esto podría ser algo así:

      CREATE TABLE film (
        film_id SERIAL PRIMARY KEY,
        title VARCHAR(255) NOT NULL,
        release_year INTEGER
      );
      
#Un ejemplo de INSERT:
 
      INSERT INTO film (title, release_year) VALUES ('The Shawshank Redemption', 1994);
      INSERT INTO film (title, release_year) VALUES ('The Godfather', 1972);
      INSERT INTO film (title, release_year) VALUES ('The Godfather: Part II', 1974);
      
      
# Un paso más con parámetros out

            create or replace function get_film_stat(
                out min_len int,
                out max_len int,
                out avg_len numeric) 
            language plpgsql
            as $$
            begin

              select min(length),
                     max(length),
                         avg(length)::numeric(5,1)
              into min_len, max_len, avg_len
              from film;

            end;$$

¿Qué hace? Explicadlo.

# Una vieja conocidad, la función swap.

Esta función intercambia dos valores, ¿verdad?

create or replace function swap(
	inout x int,
	inout y int
) 
language plpgsql	
as $$
begin
   select x,y into y,x3 ;
end; $$;

# Un ejercicio

Crear una función que reciba un parámetro numérico y devuelva el doble de ese número.

# Otro más

 Crear una función que reciba un parámetro de texto (nombre de una ciudad) y actualice la tabla "customers" estableciendo un descuento del 10% para todos los clientes que residan en esa ciudad.

# Fuente

- https://www.postgresqltutorial.com/postgresql-plpgsql/plpgsql-function-parameters/
