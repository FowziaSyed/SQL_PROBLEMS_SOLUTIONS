#Problem Statement :

Meta/Facebook (Hard Level)  Interview Question
A table named “famous” has two columns called user id and follower id. It represents each user ID has a particular follower ID. 
These follower IDs are also users of Facebook / Meta. Then, find the famous percentage of each user. 
Famous Percentage = number of followers a user has / total number of users on the platform.

#Schema and Dataset :
CREATE TABLE famous (user_id INT, follower_id INT);

INSERT INTO famous VALUES
(1, 2), (1, 3), (2, 4), (5, 1), (5, 3), 
(11, 7), (12, 8), (13, 5), (13, 10), 
(14, 12), (14, 3), (15, 14), (15, 13);

#Solution :

1. First Ihave found the distinct users available considering  the  followers as well 
2. next got the total  followers count per user 
3. final step to calculated the percentage value for each user 

#Query :

with unique_users as (
	select user_id as users from famous 
	union 
	select follower_id as users from famous 
),total_users as (
	select count(*) as userscount from unique_users
),
total_followers as (
	select user_id,count(follower_id) as followers from famous group by user_id
)select f.user_id,(f.followers*100.0)/(userscount) as "Famous Percentage"
from total_users u , total_followers f  order by f.user_id

#Output :

!(image.png)





