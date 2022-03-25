# Ejercicio 5
##### Filtrando las órdenes confirmadas, calcular a nivel usuario el porcentaje de órdenes confirmadas, que tienen entre los “Top 100” restaurantes (según cantidad de órdenes confirmadas)
```sql
WITH mytable AS (SELECT user_id, COUNT(DISTINCT id) as count_id, a.restaurant_id, b.restaurant_id
				FROM orders_sample a
				LEFT JOIN (SELECT restaurant_id, COUNT(id) as count_id 
							FROM orders_sample
							WHERE state = 'CONFIRMED'
							GROUP BY restaurant_id
							ORDER BY count_id DESC 
							LIMIT 100) b
				ON a.restaurant_id = b.restaurant_id
                WHERE a.restaurant_id = b.restaurant_id
				GROUP BY user_id
				order by user_id, count_id DESC)

SELECT a.user_id, COUNT(DISTINCT a.id) as conf_orders, IFNULL(b.count_id, 0) AS conf_orders_top_100_rest,
ROUND((IFNULL(b.count_id, 0)*1.0)/COUNT(DISTINCT a.id) * 100, 2) || '%' as ratio_con_orders_top_100
FROM orders_sample a 
LEFT JOIN mytable b
ON a.user_id=b.user_id
WHERE state = 'CONFIRMED'
GROUP BY a.user_id
ORDER BY conf_orders DESC, ratio_con_orders_top_100 DESC;
```
*Nota: Para una mayor comprensión de las columnas, para conf_orders se tomó todas las ordenes confirmadas para un user_id, y para conf_orders_top_100_rest se tomó todas las ordenes confirmadas para un user_id, solo en el TOP100 de restaurantes.*
##### Primeras 5 muestras del resultado:
![image](https://user-images.githubusercontent.com/81542475/160057464-371122bb-392d-43fd-b768-cc39df3fd199.png)

