# Ejercicio 2
##### Cantidad de restaurantes en Uruguay, por categoría del restaurante, que tienen por lo menos un monto mensual (amount_local_currency)) en marzo 2019 mayor a 100.
```sql
WITH mytable AS (SELECT DISTINCT a.restaurant_id, SUM(a.amount_local_currency) as amount_march
				FROM orders_sample a 
				INNER JOIN citiessample b ON b.city_id=a.city_id
				WHERE b.country_name = 'Uruguay' 
				AND SUBSTRING(a.order_date, 1, 7) = '2019-03'
				AND a.state = 'CONFIRMED'
				GROUP BY restaurant_id)

SELECT (case when c.business_type_name IS NULL THEN 'No Category' ELSE c.business_type_name END) AS category, count(DISTINCT a.restaurant_id) AS restaurant_qty
FROM orders_sample a 
LEFT JOIN citiessample b ON a.city_id=b.city_id
LEFT JOIN partnerssample c ON c.partner_id=a.restaurant_id
LEFT JOIN mytable d ON d.restaurant_id=a.restaurant_id
WHERE b.country_name='Uruguay'
AND a.state = 'CONFIRMED'
AND (CASE WHEN d.amount_march IS NULL THEN 0 ELSE d.amount_march ENd) >100
GROUP BY c.business_type_name
ORDER BY restaurant_qty DESC;
```
*Nota: En la columna "Category", se nombro como "No Category" a aquellas que no se le pudo encontrar una categoria en la tabla partnerssample.csv*
##### Primeras 5 muestras del resultado:
![image](https://user-images.githubusercontent.com/81542475/160055022-a82e3843-f15c-4399-a992-69f2fc8ce161.png)

