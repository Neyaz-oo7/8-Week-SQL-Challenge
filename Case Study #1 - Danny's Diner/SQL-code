set search_path=dannys_diner ;

select * from menu;
select * from members;
select * from sales;
-- What is the total amount each customer spent at the restaurant?
select * 
from sales s
inner join menu m on s.product_id=m.product_id
order by s.customer_id

-- How many days has each customer visited the restaurant?

select s.customer_id,sum(m.price) as toal_spent_by_each_customer
from sales s
inner join menu m on s.product_id=m.product_id
group by s.customer_id;

-- What was the first item from the menu purchased by each customer?
select s.customer_id,count(distinct s.order_date) as total_day_spent_by_each_customer
from sales s
inner join menu m on s.product_id=m.product_id
group by s.customer_id;
-- What is the most purchased item on the menu and how many times was it purchased by all customers?
with cte as (select * ,
dense_rank() over(partition by s.customer_id order by s.order_date asc ) as rn
from sales s
inner join menu m on s.product_id=m.product_id)

select customer_id,product_name
from cte where rn=1


select m.product_name, count(s.customer_id) as count_of_most_purchased_item 
from sales s
inner join menu m on s.product_id=m.product_id
group by m.product_name
order by count_of_most_purchased_item  desc
limit 1

-- Which item was the most popular for each customer?
with cte as (select s.customer_id,m.product_name ,count(m.product_id) as rn
from sales s
inner join menu m on s.product_id=m.product_id
group by s.customer_id,m.product_name)
select m.customer_id,m.product_name,m.rn as count_of_product from (select *,
dense_rank() over(partition by customer_id order by rn desc) as rnk
from cte) m
where m.rnk=1

-- Which item was purchased first by the customer after they became a member?
with cte as (select s.customer_id,m.product_name ,s.order_date,
row_number() over(partition by s.customer_id order by s.order_date desc ) as rn
from sales s
inner join menu m on s.product_id=m.product_id
inner join members mm on s.customer_id=mm.customer_id
where mm.join_date>s.order_date)

select customer_id ,product_name
from cte 
where rn=1


 -- Which item was purchased just before the customer became a member?

with cte as (select s.customer_id,m.product_name ,s.order_date,
row_number() over(partition by s.customer_id order by s.order_date desc ) as rn
from sales s
inner join menu m on s.product_id=m.product_id
inner join members mm on s.customer_id=mm.customer_id
where mm.join_date>s.order_date)

select customer_id ,product_name
from cte 
where rn=1


-- What is the total items and amount spent for each member before they became a member?

select s.customer_id,count(s.product_id) as total_item ,sum(m.price) as total_spent
from sales s
inner join menu m on s.product_id=m.product_id
inner join members mm on s.customer_id=mm.customer_id
where s.order_date<mm.join_date
group by s.customer_id
order by s.customer_id

-- If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

select customer_id,
sum(case 	
	when m.product_name='sushi'then m.price*20
	else m.price*10
   end ) as total_points
from sales s
inner join menu m on s.product_id=m.product_id
group by customer_id
order by customer_id









