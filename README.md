
## Project 1 MIST 4610

Team Name: 21482_3


Team Members

- Lily Fitzgerald  [@lilyjfitz](https://github.com/lilyjfitz)
- Pierre Paradis [@pierreparadis](https://github.com/pierreparadis)
- Ethan Delamater  [@ethandelamater](https://github.com/ethandelamater)
- Anders Roth [@AndersRoth](https://github.com/AndersRoth)
- Joseph Fredeman [@Jfredeman](https://github.com/Jfredeman)





## Problem Description
Our team decided to look into the car dealership industry, particularly businesses like Carmax. We chose this because we felt that there is problem in the lack of organization and management between sales, finance, and the service departments within a car dealership. Our team created Data Model to address this problem and showcase the relationships between all aspects of a car dealership. The Data Model comprises different entities that form the car dealership system as a whole, such as sales, payments, service requests, customers, salespeople, vehicles, etc. These entities are interconnected through appropriate relationships and possess detailed attributes within each category. The goal of our project is to make the car dealership process more smooth, facilitate communication between different departments, and improve customer satisfaction experience.

Improvements:
- Our first improvement is offering any service request at the car dealership at a reduced rate so the customer doesnt feel the need to find a 3rd party business. These service requests can include anything from car matienece to general detailing and cleaning. 
- Another improvement is the incorporation of a personalized salesperson. We have a head manager assuring each saleperson is doing their job well, and each customer will only ever have to deal with one salesperson. Dealing with one and only one trained professional will help reduce the customer stress and make them feel more comfortable in the experience.
## Data Model

![alt text](IMG_2283.png)

Data Model Description:

ONE TO MANY Identifying Relationships:
- A Customer can have multiple Sales but without the customer there are no Sales

ONE TO MANY Relationships:
- A Salesperson has many Sales
- A Salesperson has many Customers
- A Customer has many Payments
- A Dealership has many Sales people
- A Finance has mutiple Payments

ONE TO ONE Relationships:
- Each Sale has has one Payment plan and a Payment plan is attached to one Sale
- A Sale includes one Vehicle and a Vehicle has one sale

MANY TO MANY Relationships:
- Dealership to Manufacturer - A dealership can get vehicles from multiple manufacturers and a manufacturer will sell vehicles to multiple dealerships.
- Customer to Service- a customer can request multiple services, and a service can be requested by multiple customers. 

The Associative tables are Vehicle and Service Request.

RECURSIVE Relationship:
- Manager to Salesperson- A manager can manage multiple Sales people, but a Salesperson only has one Manager. 


🔗 [Data Dictionary](https://docs.google.com/spreadsheets/d/1QlI2LRiLOjDiEhPXp9Ho8GiUAaRuEWVMSRwXiWoqvOo/edit#gid=0)
## Queries
🔗 [Query Matrix](https://docs.google.com/spreadsheets/d/1QlI2LRiLOjDiEhPXp9Ho8GiUAaRuEWVMSRwXiWoqvOo/edit#gid=1446005263)
- Query 1:
Description: List the different cities of dealerships, count of the number of salespeople in that city, the number of sales that salespeople at that office were sales representatives for, and total sales. 

Justification: A manager would like to know which dealerships are most profitable, as well as the relation between sales and number of salespeople per dealership. It will help them with potential dealership expansions and investment in more profitable dealerships

SELECT Dealership.city, COUNT(DISTINCT Salesperson.spID) AS "Number of Salespeople", COUNT(saleNumber) AS "Number of Sales", SUM(saleAmount) AS "Total Sales" 
FROM Dealership 
JOIN Salesperson ON Dealership.dealershipID = Salesperson.dealershipID JOIN Sale ON Salesperson.spID = Sale.spID 
GROUP BY Dealership.city;

Result:
![alt text](IMG_8833.png)
- Query 2:
- Query 3:
- Query 4:
- Query 5:
- Query 6:
- Query 7:
- Query 8:
- Query 9:
- Query 10:
## Database Information
