# Consultas jardineria 😎

## 1.4.5 Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```sql
    SELECT c.nombre_cliente AS Nombre_cliente, e.nombre AS Nombre_rep_ventas, e.apellido1 AS Apellido_rep_ventas FROM cliente c INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
```

```sql
    SELECT c.nombre_cliente AS Nombre_cliente,e.nombre AS Nombre_rep_ventas,e.apellido1 AS Apellido_rep_ventas FROM cliente c,empleado e WHERE c.codigo_empleado_rep_ventas = e.codigo_empleado;

```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
    SELECT DISTINCT c.codigo_cliente AS COD_Cliente, c.nombre_cliente AS Nombre_cliente, CONCAT( e.nombre,' ',e.apellido1,' ',e.apellido2) AS Representante_ventas FROM cliente c INNER JOIN empleado e  ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente ;

```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
    SELECT c.codigo_cliente, c.nombre_cliente ,e.codigo_empleado, CONCAT(e.nombre,' ',e.apellido1,' ',e.apellido2 ) AS REPRESENTANTE_VENTAS FROM cliente c LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado WHERE p.codigo_cliente IS NULL;
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
     SELECT DISTINCT c.codigo_cliente , c.nombre_cliente AS Cliente , e.codigo_empleado, CONCAT (e.nombre,' ',e.apellido1) AS empleado , o.ciudad AS ciudad_Representante_Ventas  FROM cliente c INNER
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
    SELECT c.nombre_cliente , CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2)AS representante_ventas , o.ciudad FROM cliente c LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina WHERE p.codigo_cliente IS NULL;
```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

```sql
     SELECT DISTINCT o.linea_direccion1 AS OFICINA_DIRECCIÓN FROM oficina o INNER JOIN empleado e ON o.codigo_oficina = e.codigo_oficina INNER JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas WHERE c.ciudad = 'Fuenlabrada';
```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
    SELECT c.nombre_cliente , CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2)AS representante_ventas , o.ciudad AS Ciudad_Representante FROM cliente c INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina ;
```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

```sql
    SELECT e.nombre AS EMPLEADO, ej.nombre AS JEFE FROM empleado e LEFT JOIN empleado ej ON e.codigo_jefe = ej.codigo_empleado;
```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

```sql
    SELECT e.nombre AS EMPLEADO, ej.nombre AS JEFE ,ejj.nombre AS JEFE_DEL_JEFE FROM empleado e LEFT JOIN empleado ej ON e.codigo_jefe = ej.codigo_empleado LEFT JOIN empleado ejj ON ej.codigo_jefe = ejj.codigo_empleado;
```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

```sql
    /* primera opción agregando las fechas de entrega que aparecen como NULL */

    SELECT DISTINCT c.nombre_cliente FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE p.fecha_entrega > p.fecha_esperada;
    
    /* segunda opción agregando las fechas de entrega que aparecen como NULL */
    SELECT DISTINCT c.nombre_cliente, p.fecha_esperada, p.fecha_entrega, p.estado, p.comentarios FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE (p.fecha_entrega > p.fecha_esperada) OR ((p.fecha_entrega IS NULL) AND (p.estado = 'Entregado'));
    
    /* tercera opción (del profe Miguel para analizar si le falta algo más a la consulta) */
    SELECT DISTINCT c.nombre_cliente, p.fecha_esperada, p.fecha_entrega, p.estado, p.comentarios FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE p.estado = 'Pendiente';
```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

```sql
    SELECT DISTINCT c.nombre_cliente , g.gama FROM gama_producto g INNER JOIN producto p ON g.gama = p.gama INNER JOIN detalle_pedido d ON p.codigo_producto = d.codigo_producto INNER JOIN pedido pd ON d.codigo_pedido = pd.codigo_pedido INNER JOIN cliente c ON pd.codigo_cliente = c.codigo_cliente;
```

## 1.4.6 Consultas multitabla (Composición externa)

**Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN y NATURAL RIGHT JOIN.**


1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
 
 ```sql
    SELECT c.codigo_cliente AS CODIGO, c.nombre_cliente AS CLIENTE FROM cliente c LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente WHERE p.codigo_cliente IS NULL;
 ```
 
2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.
 
 ```sql
    SELECT c.codigo_cliente AS CODIGO, c.nombre_cliente AS CLIENTE FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE p.codigo_cliente IS NULL;
 ```
 
3. Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.
 
 ```sql
    SELECT c.codigo_cliente AS CODIGO, c.nombre_cliente AS CLIENTE FROM cliente c LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente LEFT JOIN pedido pd ON c.codigo_cliente = pd.codigo_cliente WHERE p.codigo_cliente IS NULL AND pd.codigo_cliente IS NULL;
 ```
 
4. Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.
 
 ```sql
    SELECT e.codigo_empleado AS CODIGO , CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2) AS EMPLEADO FROM empleado e LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina WHERE e.codigo_oficina IS NULL;
 ```
 
5. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.
 
 ```sql
    SELECT e.codigo_empleado AS CODIGO, CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2) AS EMPLEADO FROM empleado e LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas WHERE c.codigo_empleado_rep_ventas IS NULL ;
 ```
 
6. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado junto con los datos de la oficina donde trabajan.
 
 ```sql
    SELECT e.codigo_empleado AS CODIGO, CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2) AS EMPLEADO , o.telefono AS TELEFONO_OFICINA, o.ciudad AS CIUDAD, o.pais AS PAÍS FROM empleado e LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina  WHERE c.codigo_empleado_rep_ventas IS NULL ;
 ```
 
7. Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.
 
 ```sql
    SELECT CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2) AS EMPLEADO FROM empleado e LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas WHERE e.codigo_oficina IS NULL AND c.codigo_empleado_rep_ventas IS NULL;
 ```
 
8. Devuelve un listado de los productos que nunca han aparecido en un pedido.
 
 ```sql
    SELECT p.codigo_producto AS CÓDIGO ,p.nombre AS PRODUCTO FROM producto p LEFT JOIN detalle_pedido d ON p.codigo_producto = d.codigo_producto WHERE d.codigo_producto IS  NULL;
 ```
 
9. Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto.
 
 ```sql
    SELECT p.codigo_producto AS CÓDIGO ,p.nombre AS PRODUCTO ,p.descripcion AS DESCRIPCIÓN , g.imagen AS IMG_GAMA FROM producto p LEFT JOIN detalle_pedido d ON p.codigo_producto = d.codigo_producto INNER JOIN gama_producto g ON p.gama = g.gama WHERE d.codigo_producto IS  NULL;
 ```
 
10. Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.
 
 ```sql
    
    *

    select distinct o.codigo_oficina, e.nombre
    from oficina o
    left join empleado e on o.codigo_oficina = e.codigo_oficina
    left join cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas
    left join pedido p on c.codigo_cliente = p.codigo_cliente
    left join detalle_pedido dp on p.codigo_pedido = dp.codigo_pedido
    left join producto pr ON dp.codigo_producto = pr.codigo_producto
    where pr.gama = 'Frutales' and c.codigo_empleado_rep_ventas is not null
    and e.codigo_empleado is not null
    and c.codigo_cliente is not null
    and p.codigo_pedido is not null
    and dp.codigo_pedido is not null
    and pr.codigo_producto is not null
    and o.codigo_oficina is not null;

 ```
 
11. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.
 
 ```sql
    SELECT DISTINCT c.codigo_cliente AS CODIGO,c.nombre_cliente AS CLIENTE
    FROM cliente c
    INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    LEFT JOIN pago pg ON c.codigo_cliente = pg.codigo_cliente
    WHERE pg.codigo_cliente IS NULL;
 ```
 
12. Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.
 
 ```sql
    SELECT e.codigo_empleado AS CÓDIGO_EMPLEADO , 
        CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2) AS EMPLEADO
    FROM empleado e
    LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    WHERE c.codigo_empleado_rep_ventas IS NULL;
 ```
 
# CONSULTAS 3


1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

```sql
   SELECT o.codigo_oficina AS CODIGO,
   o.ciudad AS CIUDAD
   FROM oficina o
   ORDER BY o.ciudad ASC;
```

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

```sql
   SELECT ciudad, telefono 
   FROM oficina
   WHERE pais = 'España';

```

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7.

```sql
   SELECT CONCAT (nombre,' ',apellido1,' ',apellido2) AS EMPLEADO,
   email AS CORREO
   FROM empleado
   WHERE codigo_jefe = 7;
   ```

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.

```sql
   SELECT puesto AS CARGO,
      CONCAT (nombre,' ',apellido1,' ',apellido2) AS NOMBRE,
      email AS CORREO 
   FROM  empleado
   WHERE codigo_jefe IS NULL;
```

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.

```sql
   SELECT CONCAT (nombre,' ',apellido1,' ',apellido2) AS NOMBRE,
      puesto AS CARGO
   FROM empleado
   WHERE NOT puesto = 'Representante Ventas';
```

6. Devuelve un listado con el nombre de los todos los clientes españoles.

```sql
   SELECT codigo_cliente AS CODIGO,
   nombre_cliente AS NOMBRE_CLIENTE,
   pais AS PAÍS
   FROM cliente 
   WHERE pais = 'Spain';
```

7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.

```sql
   SELECT DISTINCT estado 
   FROM pedido;

```

8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:

* Utilizando la función `YEAR de MySQL`.
* Utilizando la función `DATE_FORMAT de MySQL``.
* Sin utilizar ninguna de las funciones anteriores.

```sql
   SELECT c.codigo_cliente, MIN(p.fecha_pago) AS Fecha_pago
   FROM cliente c
   INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente
   WHERE YEAR(p.fecha_pago) = 2008
   GROUP BY c.codigo_cliente;

   SELECT c.codigo_cliente AS CODÍGO_CLIENTE, MIN(p.fecha_pago) AS FECHA_PAGO
   FROM cliente c
   INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente
   WHERE DATE_FORMAT(p.fecha_pago,'%Y') = 2008
   GROUP BY c.codigo_cliente;
   
   SELECT  c.codigo_cliente, MIN(p.fecha_pago) AS Fecha_pago
   FROM cliente c
   INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente
   WHERE p.fecha_pago BETWEEN '2008-01-01' AND '2008-12-31'
   GROUP BY c.codigo_cliente;

```

9. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.

```sql
   SELECT p.codigo_pedido,p.codigo_cliente,p.fecha_esperada,p.fecha_entrega 
   FROM pedido p
   WHERE p.fecha_entrega > p.fecha_esperada;
```

10. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.

* Utilizando la función `ADDDATE de MySQL`.
* Utilizando la función `DATEDIFF de MySQL`.
* ¿Sería posible resolver esta consulta utilizando el operador de suma + o resta -?

```sql
    
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega 
FROM pedido 
WHERE fecha_entrega <= ADDDATE(fecha_esperada, INTERVAL -2 DAY);


SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega 
FROM pedido 
WHERE DATEDIFF(fecha_entrega,fecha_esperada) <= -2;


SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega 
FROM pedido 
WHERE fecha_entrega <= fecha_esperada - INTERVAL 2 DAY;
```

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

```sql
   SELECT codigo_pedido,fecha_pedido,estado
   FROM pedido
   WHERE YEAR(fecha_pedido) = 2009 AND estado = 'Rechazado'
   ORDER BY fecha_pedido;
```

12. Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.

```sql
   SELECT codigo_pedido,fecha_pedido,estado
   FROM pedido
   WHERE MONTH(fecha_pedido) = 01 AND estado = 'Entregado'
   ORDER BY fecha_pedido;
```

13. Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

```sql
   SELECT id_transaccion,fecha_pago AS año,forma_pago
   FROM pago
   WHERE YEAR(fecha_pago) = 2008 AND forma_pago = 'PayPal'
   ORDER BY fecha_pago;
```

14. Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.

```sql
   SELECT forma_pago FROM pago  GROUP BY forma_pago;
```

15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.

```sql
   SELECT codigo_producto,gama,cantidad_en_stock,precio_venta
   FROM producto
   WHERE gama = 'Ornamentales' AND cantidad_en_stock > 100
   ORDER BY precio_venta DESC;
```

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el código de empleado 11 o 30.

```sql
   SELECT codigo_cliente AS Codigo,
    nombre_cliente AS Nombre,
    ciudad AS Ciudad,
    codigo_empleado_rep_ventas AS Representante
   FROM cliente
   WHERE  ciudad = 'Madrid' AND ( codigo_empleado_rep_ventas = 11 OR codigo_empleado_rep_ventas = 30 );
```



# SUBCONSULTAS
### 1.4.8 Subconsultas

#### 1.4.8.1 Con operadores básicos de comparación

1. Devuelve el nombre del cliente con mayor límite de crédito.
```sql
SELECT nombre_cliente 
FROM cliente  
WHERE limite_credito = (SELECT MAX(limite_credito) FROM cliente);
```
2. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql
SELECT nombre,precio_venta
FROM producto
WHERE precio_venta = (SELECT MAX(precio_venta) FROM producto);
```
3. Devuelve el nombre del producto del que se han vendido más unidades. (Tenga en cuenta que tendrá que calcular cuál es el número total de unidades que se han vendido de cada producto a partir de los datos de la tabla `detalle_pedido`)

```sql

SELECT nombre 
FROM producto 
WHERE codigo_producto = (   SELECT codigo_producto 
                            FROM detalle_pedido 
                            GROUP BY codigo_producto 
                            ORDER BY SUM(cantidad) 
                            DESC LIMIT 1
                        );
```
4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya realizado. (Sin utilizar `INNER JOIN`).

```sql
   SELECT *
   FROM cliente
   WHERE   limite_credito > 
                           (
                              SELECT SUM(total)
                              FROM pago
                              WHERE cliente.codigo_cliente = pago.codigo_cliente
                           );
```
5. Devuelve el producto que más unidades tiene en stock.
```sql

SELECT nombre,cantidad_en_stock
FROM producto
WHERE cantidad_en_stock = ( SELECT MAX(cantidad_en_stock)
                            FROM producto 
                            );
```
6. Devuelve el producto que menos unidades tiene en stock.
```sql
SELECT nombre,cantidad_en_stock
FROM producto
WHERE cantidad_en_stock =   (
                                SELECT MIN(cantidad_en_stock)
                                FROM producto
                            );
```
7. Devuelve el nombre, los apellidos y el email de los empleados que están a cargo de **Alberto Soria**.
```sql
SELECT nombre,apellido1,apellido2,email
FROM empleado
WHERE codigo_jefe = 3;
```
 

#### 1.4.8.2 Subconsultas con ALL y ANY

1. Devuelve el nombre del cliente con mayor límite de crédito.
```sql
   SELECT nombre_cliente
   FROM cliente
   WHERE limite_credito >= ALL( SELECT limite_credito FROM cliente);
```
2. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql
   SELECT nombre
   FROM producto
   WHERE precio_venta >= ALL ( SELECT precio_venta FROM producto);
```
3. Devuelve el producto que menos unidades tiene en stock.
```sql
   SELECT *
   FROM producto
   WHERE cantidad_en_stock <= ALL ( SELECT cantidad_en_stock FROM producto);
```


#### 1.4.8.3 Subconsultas con IN y NOT IN

1. Devuelve el nombre, apellido1 y cargo de los empleados que no representen a ningún cliente.
```sql
   select codigo_empleado,nombre,apellido1 as apellido,puesto as cargo 
   from empleado
   where codigo_empleado not in (select codigo_empleado_rep_ventas from cliente );
```
2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql
   SELECT * 
   FROM cliente
   WHERE codigo_cliente NOT IN (
                                 SELECT codigo_cliente 
                                 FROM pago 
                                 GROUP BY codigo_cliente);
```
3. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql
   SELECT * 
   FROM cliente
   WHERE codigo_cliente IN (   
                              SELECT codigo_cliente 
                              FROM pago 
                              GROUP BY codigo_cliente);
      
```
4. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql
   SELECT *
   FROM producto
   WHERE codigo_producto NOT IN (
                                 SELECT codigo_producto
                                 FROM detalle_pedido
                                 GROUP BY codigo_producto);
```
5. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.
```sql
   SELECT e.nombre Nombres , CONCAT(e.apellido1,' ',e.apellido2) Apellidos ,e.puesto Puesto, o.telefono Telefono_Oficina
   FROM empleado e
   LEFT JOIN oficina o ON o.codigo_oficina = e.codigo_oficina 
   WHERE e.codigo_empleado NOT IN (
   SELECT codigo_empleado_rep_ventas
   FROM cliente);
```
6. Devuelve las oficinas donde **no trabajan** ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama `Frutales`.
```sql
   SELECT *
   FROM oficina 
   WHERE codigo_oficina NOT IN (SELECT id_oficina FROM (SELECT c.codigo_empleado_rep_ventas id_empleado, em.codigo_oficina id_oficina
   FROM cliente c 
   INNER JOIN empleado em ON c.codigo_empleado_rep_ventas = em.codigo_empleado
   INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
   INNER JOIN detalle_pedido dp ON p.codigo_pedido = dp.codigo_pedido
   INNER JOIN producto pr ON dp.codigo_producto = pr.codigo_producto
   WHERE pr.gama = 'Frutales'
   GROUP BY c.codigo_empleado_rep_ventas 
   ORDER BY c.codigo_empleado_rep_ventas) oficinas_rep_ventas);
```
7. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.
```sql
   SELECT *
   FROM cliente
   WHERE 
   (codigo_cliente IN (SELECT codigo_cliente FROM pedido)) AND (codigo_cliente NOT IN (SELECT codigo_cliente FROM pago));
```
 
#### 1.4.8.4 Subconsultas con EXISTS y NOT EXISTS

1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql
   SELECT *
   FROM cliente c
   WHERE NOT EXISTS ( SELECT 1 
                     FROM pago p 
                     WHERE c.codigo_cliente = p.codigo_cliente );
```
2. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql
   SELECT *
   FROM cliente c
   WHERE EXISTS (  SELECT 1
                  FROM pago p
                  WHERE c.codigo_cliente = p.codigo_cliente);
```
3. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql
   SELECT *
   FROM producto p
   WHERE NOT EXISTS (  SELECT 1
                     FROM detalle_pedido dp
                     WHERE p.codigo_producto = dp.codigo_producto);
```
4. Devuelve un listado de los productos que han aparecido en un pedido alguna vez.
```sql
   SELECT *
   FROM producto p
   WHERE EXISTS (
                  SELECT 1
                  FROM detalle_pedido dp
                  WHERE p.codigo_producto = dp.codigo_producto);
```


# TIPS

### 5 TIPS con SELECT en SQL

<details>
  <summary>Click para desplegar</summary>
  <br>


   1. Valores fijo
   ```sql
      SELECT codigo_cliente,fecha_pago, 'Desconocido' cajero
      FROM pago
      LIMIT 5;
   ```
   2. operaciones con columna
   concatenar o operar valores numericos

   ```sql
      SELECT codigo_producto,cantidad,precio_unidad,(cantidad * precio_unidad) as Total, CONCAT(codigo_producto,'-Linea-',numero_linea) as Descripcion
      FROM detalle_pedido
      LIMIT 5;
   ```

   3. condiciones 

   Segun el valor de una columna , darle valor a otra columna inventada
   ```sql
      SELECT nombre,gama,
      CASE 
      WHEN gama = 'Aromáticas' THEN 'Producto Aromatico'
      WHEN gama = 'Herramientas' THEN 'Producto de trabajo'
      END tipo
      FROM producto; 
   ```

   4. subconsultas

   hacer consultas sin unsar INNER JOIN (contar cuantos productos existen por gama)
   ```sql
      SELECT gama,(select COUNT(producto.nombre ) FROM producto WHERE producto.gama = gama_producto.gama ) cant_productos
      FROM gama_producto;   
   ```
   5. consulta sobre sub consulta

   hacer una consulta de una tabla que es resultado de otra consulta

   ```sql
      SELECT gama,cant_productos
      FROM (  SELECT gama,(   select COUNT(producto.nombre ) 
                              FROM producto 
                              WHERE producto.gama = gama_producto.gama ) cant_productos
            FROM gama_producto) as mi_tabla
      WHERE cant_productos < 20;
   ```
   6. columna auto incrementable VIRTUAL
   (ROW_NUMBER)

   enumera las filas resultantea de la consulta
   ```sql
      SELECT  ROW_NUMBER () OVER(ORDER BY(SELECT 1)) my_id_virtual,
      gama, descripcion_texto
      FROM gama_producto;   
   ```
   
</details>

### 5 TIPS con WHERE en SQL

<details>
  <summary>Click para desplegar</summary>
   <br>

   1. NOT IN
   ```sql
      SELECT *
      FROM gama_producto
      WHERE gama NOT IN ('Herbaceas','Herramientas');
   ```

   2. Subconsulta
   ```sql
      SELECT *
      FROM gama_producto gm
      WHERE (
         SELECT COUNT(*)
         FROM producto pr
         WHERE gm.gama = pr.gama
      ) > 0;
   ```


   3. regex

   ```sql
      SELECT *
      FROM gama_producto
      WHERE gama LIKE 'A%';
   ```

   4. IN Y SUBCONSULTAS
   ```sql
      SELECT codigo_producto,nombre,precio_venta,gama
      FROM producto
      WHERE gama IN (
                        SELECT gama
                        FROM gama_producto
                        WHERE gama LIKE 'H%'
      );
   ```

   5. FUNCIONES
   ```sql
      SELECT *
      FROM gama_producto
      WHERE SUBSTRING(gama,1,1) = 'H';
   ```
</details>

### 5 TIPS con GROUP BY en SQL


<details>
  <summary>Click para desplegar</summary>
   <br>

   1. 
   ```sql
      
   ```

   2. 
   ```sql
      
   ```


   3. 
   ```sql
      
   ```

   4. 
   ```sql
      
   ```

   5. 
   ```sql
      
   ```
</details>

### 5 TIPS con UPDATE en SQL


<details>
  <summary>Click para desplegar</summary>
   <br>

   1. 
   ```sql
      
   ```

   2. 
   ```sql
      
   ```


   3. 
   ```sql
      
   ```

   4. 
   ```sql
      
   ```

   5. 
   ```sql
      
   ```
</details>