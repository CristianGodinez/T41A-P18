# Clase: PostgreSQL como NoSQL - Arrays y CTE Recursivas para Modelar Grafos

## Introducción
PostgreSQL es una base de datos relacional que también ofrece capacidades NoSQL, como almacenamiento de datos en formato JSON, arrays y consultas recursivas. Estas características permiten modelar estructuras complejas como grafos sin necesidad de sistemas externos.

---

## 1. Arrays en PostgreSQL
Los arrays permiten almacenar múltiples valores en una sola columna, lo que es útil para representar listas o colecciones.

### Teoría
- Un array en PostgreSQL puede ser unidimensional o multidimensional.
- Se declara con `tipo[]`, por ejemplo: `INTEGER[]`.

### Ejemplo
```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nombre TEXT,
    intereses TEXT[]
);

INSERT INTO usuarios (nombre, intereses)
VALUES ('Ana', ARRAY['PostgreSQL', 'Grafos', 'NoSQL']);

SELECT nombre, intereses[1] AS primer_interes FROM usuarios;
```

### Ejercicio Propuesto
1. Crear una tabla `productos` con un array de etiquetas.
2. Insertar al menos 3 productos con diferentes etiquetas.
3. Consultar todos los productos que contengan la etiqueta 'tecnología'.

---

## 2. CTE Recursivas para Modelar Grafos
Las CTE (Common Table Expressions) recursivas permiten recorrer estructuras jerárquicas o grafos.

### Teoría
- Una CTE recursiva tiene dos partes: la base y la recursión.
- Se utiliza para recorrer nodos conectados mediante relaciones.

### Ejemplo: Grafo de Amigos
```sql
CREATE TABLE amigos (
    id SERIAL PRIMARY KEY,
    nombre TEXT,
    amigo_id INT
);

INSERT INTO amigos (nombre, amigo_id) VALUES
('Ana', NULL),
('Luis', 1),
('Marta', 2),
('Pedro', 2);

WITH RECURSIVE red_amigos AS (
    SELECT id, nombre, amigo_id FROM amigos WHERE nombre = 'Ana'
    UNION ALL
    SELECT a.id, a.nombre, a.amigo_id
    FROM amigos a
    INNER JOIN red_amigos r ON a.amigo_id = r.id
)
SELECT * FROM red_amigos;
```

### Ejercicio Propuesto
1. Crear una tabla `empleados` con columnas `id`, `nombre`, `jefe_id`.
2. Insertar una jerarquía de empleados.
3. Usar una CTE recursiva para listar todos los subordinados de un jefe específico.

---

## Ejercicios Recomendados
- Modelar un grafo de ciudades conectadas por rutas usando CTE recursivas.
- Crear una tabla con arrays que representen las conexiones directas de cada ciudad.
- Consultar todas las ciudades alcanzables desde una ciudad inicial.

---

## Recursos
- [Documentación oficial de PostgreSQL](https://www.postgresql.org/docs/)
- [CTE Recursivas](https://www.postgresql.org/docs/current/queries-with.html)
- [Arrays en PostgreSQL](https://www.postgresql.org/docs/current/arrays.html)

