# Ejercicio 1
##### Cantidad de órdenes y monto total de las mismas (local y en dólares), por ciudad, país y año-mes.
```sql
WITH mytable AS (SELECT rate_us, SUBSTRING(currency_exchange_date,1,4) || '' || SUBSTRING(currency_exchange_date,6,2) as date_ex, currency_id 
                 FROM currency)

SELECT c.country_name AS country, b.city_name AS city, (SUBSTRING(b.order_date,1,4) || '' || SUBSTRING(b.order_date,6,2)) AS yearmonth, 
COUNT (b.id) AS orders_qty, 
ROUND(SUM(b.amount_local_currency),2) as orders_amount_local,
ROUND((SUM(b.amount_local_currency) / e.rate_us),2) AS orders_amount_usd
FROM orders_sample b 
LEFT JOIN citiessample c ON b.city_id = c.city_id
LEFT JOIN country_currency_map d ON c.country_id = d.country_id
LEFT JOIN mytable e ON e.date_ex=yearmonth AND d.currency_id=e.currency_id
WHERE b.state = 'CONFIRMED'
GROUP BY c.country_name, b.city_name, yearmonth
ORDER BY c.country_name, yearmonth, orders_amount_usd DESC, orders_qty DESC;
```
*Nota: asdadasdas*
##### Primeras 5 muestras del resultado:
![image](https://user-images.githubusercontent.com/81542475/160052737-b1e514f6-eb5a-48c7-8e20-9a9df9d09b64.png)
