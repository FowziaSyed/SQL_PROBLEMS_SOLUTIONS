******Problem Statement :******

Uber(Hard Level) SQL Interview Question — Solution

Some forecasting methods are extremely simple and surprisingly effective. Naïve forecast is one of them. To create a naïve forecast for "distance per dollar" (defined as distance_to_travel/monetary_cost), first sum the "distance to travel" and "monetary cost" values monthly. This gives the actual value for the current month. For the forecasted value, use the previous month's value. After obtaining both actual and forecasted values, calculate the root mean squared error (RMSE) using the formula RMSE = sqrt(mean(square(actual - forecast))). Report the RMSE rounded to two decimal places.

******Schema and Dataset :******

CREATE TABLE uber_request_logs(request_id int, request_date datetime, request_status varchar(10), distance_to_travel float, 
monetary_cost float, driver_to_client_distance float);

INSERT INTO uber_request_logs VALUES (1,'2020-01-09','success', 70.59, 6.56,14.36), (2,'2020-01-24','success', 93.36, 22.68,19.9), (3,'2020-02-08','fail', 51.24, 11.39,21.32), (4,'2020-02-23','success', 61.58,8.04,44.26), (5,'2020-03-09','success', 25.04,7.19,1.74), (6,'2020-03-24','fail', 45.57, 4.68,24.19), (7,'2020-04-08','success', 24.45,12.69,15.91), (8,'2020-04-23','success', 48.22,11.2,48.82), (9,'2020-05-08','success', 56.63,4.04,16.08), (10,'2020-05-23','fail', 19.03,16.65,11.22), (11,'2020-06-07','fail', 81,6.56,26.6), (12,'2020-06-22','fail', 21.32,8.86,28.57), (13,'2020-07-07','fail', 14.74,17.76,19.33), (14,'2020-07-22','success',66.73,13.68,14.07), (15,'2020-08-06','success',32.98,16.17,25.34), (16,'2020-08-21','success',46.49,1.84,41.9), (17,'2020-09-05','fail', 45.98,12.2,2.46), (18,'2020-09-20','success',3.14,24.8,36.6), (19,'2020-10-05','success',75.33,23.04,29.99), (20,'2020-10-20','success', 53.76,22.94,18.74);

******Solution :******

1. calculated the total distance and total cost values monthwise
2. calculated the distance per dollar (total distance/total cost) as actual monthly values
3. used the lag function to obtain the forecast values 
4. performed the root mean square error formula.


******Query :******

```SQL
with  total_dis_cost as(
	select to_char(request_date,'YYYY-MM') as requestdate , 
	sum(distance_to_travel) as total_distance_traveled,sum(monetary_cost) as total_monetary_cost 
	from uber_request_logs
	group by requestdate order by requestdate
), 
actual_values as (
	select requestdate,(total_distance_traveled/total_monetary_cost) as actual_value from total_dis_cost
),
forecast_values as (
	select * , lag(actual_value) over(order by requestdate) as forecast_value from actual_values 
)
select 
round(cast(sqrt(avg((actual_value-forecast_value)*(actual_value-forecast_value))) as numeric),2) as RMSE 
from forecast_values
```
******Output :******



