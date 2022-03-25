# Ejercicio 3
##### Usando todas las órdenes en la base, calcular el promedio de días entre una orden y la siguiente a nivel usuario, solo para usuarios con al menos 5 órdenes totales. De ese resultado, quedarse solo con usuarios con un promedio mayor a 5días.
```sql
WITH mytable AS (SELECT user_id, id
				,IFNULL(Cast ((JulianDay(SUBSTRING(order_date,1,10)) - JulianDay(LAG(SUBSTRING(order_date,1,10)) OVER(PARTITION BY user_id ORDER BY SUBSTRING(order_date,1,10)))) As Integer), 0) AS previous_order
				FROM orders_sample
				ORDER BY user_id DESC, order_date)


SELECT user_id, COUNT(DISTINCT id) AS total_orders,
ROUND( AVG (CASE WHEN previous_order <> 0 THEN previous_order ELSE NULL END), 2) AS avg_days_between_orders
FROM mytable
group BY user_id
HAVING total_orders >= 5 AND avg_days_between_orders > 5
order by total_orders DESC, avg_days_between_orders DESC;
```
*Nota: Para este ejercicio como ningún user_id, tuvo al menos 5 órdenes el resultado de la query es 0. No obstante, si se quiere ver los user_id que tuvieron al menos de 2 o n cantidad de órdenes, se puede hacer, cambiando la línea de código 11*
##### Primeras 5 muestras del resultado teniendo en cuenta 2 o mas órdenes:
![image](https://user-images.githubusercontent.com/81542475/160055570-58cd9d79-8890-43c4-9b56-0ae3eae0b69c.png)
