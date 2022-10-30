-- Write a query to Select everything from sales table
select * from sales;


-- Write a query to  SELECT just a few columns from sales table
select SaleDate, Amount, Customers from sales;
select Amount, Customers, GeoID from sales;


-- Write a query to add a calculated column of amount/ quantity in sales table
Select SaleDate, Amount, Boxes, Amount divided by boxes  from sales;


-- Write a query to rename calculated column as amount per box
Select SaleDate, Amount, Boxes, Amount / boxes as 'Amount per box'  from sales;


-- Write a query to show sales data where amount is greater than 10000
select * from sales
where amount > 10000;


-- Write a query to show sales data where amount is greater than 10,000 by descending order
select * from sales
where amount > 10000
order by amount desc;

-- Write a query showing sales data where geography is g1 by product ID & descending order of amounts
select * from sales
where geoid='g1'
order by PID, Amount desc;

-- Write a query to show sales data of 01-01-2022 where amount is greater than 10000
Select * from sales
where amount > 10000 and SaleDate >= '2022-01-01';

-- Write a query to show sales for year 2022 in descending order where amount is greater than 10000
select SaleDate, Amount from sales
where amount > 10000 and year(SaleDate) = 2022
order by amount desc;

-- Write a query to show sales where boxes sold is within 0 to 50
select * from sales
where boxes >0 and boxes <=50;

select * from sales
where boxes between 0 and 50;

-- Write a query to select all everything from People table
select * from people;

-- Write a query to show all data of team delish and jucies from people table
select * from people
where team = 'Delish' or team = 'Jucies';

select * from people
where team in ('Delish','Jucies');

-- Write a query to show all data where name of salesperson starts with B
select * from people
where salesperson like 'B%';

-- Write a query to show all data where name of salesperson has B in any position
select * from people
where salesperson like '%B%';


-- Write a query to show saledate, amount, and calculated column showing whether the amount is below 1000, below 5000 or below 10000
select 	SaleDate, Amount,
		case 	when amount < 1000 then 'Under 1k'
				when amount < 5000 then 'Under 5k'
                when amount < 10000 then 'Under 10k'
			else '10k or more'
		end as 'Amount category'
from sales;

-- Write a query to show the number of salesperson in each team
select team, count(*) from people
group by team

-- Print details of shipments (sales) where amounts are > 2,000 and boxes are <100?

select * from sales where amount > 2000 and boxes < 100;

-- How many shipments (sales) each of the sales persons had in the month of January 2022?

select  s.SaleDate, p.Salesperson ,count(*) as 'Shipment Count'
from sales s 
join people p on p.SPID = s.SPID
where s.SaleDate between '2022-1-1' and '2022-1-31'
group by p.Salesperson ;

-- Which product sells more boxes? Milk Bars or Eclairs?

select pr.product, sum(s.Boxes) as 'Total Boxes'
from sales s
join products pr on s.PID= pr.PID
where pr.Product in ('Milk Bars', 'Eclairs')
group by pr.product;

-- Which product sold more boxes in the first 7 days of February 2022? Milk Bars or Eclairs?

select pr.product, sum(boxes) as 'Total Boxes'
from sales s
join products pr on s.pid = pr.pid
where pr.Product in ('Milk Bars', 'Eclairs')
and s.saledate between '2022-2-1' and '2022-2-7'
group by pr.product;

-- Which shipments had under 100 customers & under 100 boxes? Did any of them occur on Wednesday?

select * from sales
where customers < 100 and boxes < 100;

select *,
case when weekday(saledate)=2 then 'Wednesday Shipment'
else ' '
end as 'W Shipment'
from sales
where customers < 100 and boxes < 100;

-- What are the names of salespersons who had at least one shipment (sale) in the first 7 days of January 2022?

select distinct p.Salesperson
from sales s
join people p on p.spid = s.SPID
where s.SaleDate between '2022-01-01' and '2022-01-07';

-- Which salespersons did not make any shipments in the first 7 days of January 2022?

select p.salesperson
from people p
where p.spid not in
(select distinct s.spid from sales s where s.SaleDate between '2022-01-01' and '2022-01-07');
