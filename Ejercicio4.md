# Ejercicio 4
##### 4.Tiempo de entrega promedio (en min) en marzo de 2019, por país. (Tiempo de entrega: esla diferencia entre el delivered_date y order_date.)
```sql
SELECT b.country_name,
IFNULL(
  ROUND (AVG (CASE WHEN (Cast (((JulianDay(a.delivery_date)) - (JulianDay(a.order_date))) * 24 * 60 As Integer)) <> 0 THEN (Cast (((JulianDay(a.delivery_date)) - (JulianDay(a.order_date))) * 24 * 60 As Integer)) ELSE NULL END), 2), 0) AS avg_delivery_time
FROM orders_sample a 
LEFT JOIN citiessample b on a.city_id=b.city_id
WHERE a.state = 'CONFIRMED'
AND SUBSTRING(a.order_date,1,7) = '2019-03'
GROUP BY b.country_name
ORDER BY avg_delivery_time DESC;;
```
*Nota: Es importante resaltar que dentro de la tabla orders_sample.csv de las 25000 muestras, solo 652 tienen order_date diferente de delivery_date (O sea se puede calcular un tiempo de entrega diferente que 0), por lo que los resultados de los promedios pueden estar sesgados. En el caso de Bolivia, da 0 ya que todas las muestras tenían un order_date igual a delivery_date.*
##### Primeras 5 muestras del resultado:
![image](https://user-images.githubusercontent.com/81542475/160056445-cfa0b36a-2bbf-49db-baae-1e8bb938501a.png)
