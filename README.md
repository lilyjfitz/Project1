
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


_Improvements_
- Our first improvement is offering any service request at the car dealership at a reduced rate so the customer doesnt feel the need to find a 3rd party business. These service requests can include anything from car matienece to general detailing and cleaning. 
- Another improvement is the incorporation of a personalized salesperson. We have a head manager assuring each saleperson is doing their job well, and each customer will only ever have to deal with one salesperson. Dealing with one and only one trained professional will help reduce the customer stress and make them feel more comfortable in the experience.
## Data Model

![Screenshot 2023-03-31 at 11 42 53 AM](https://user-images.githubusercontent.com/126897383/229170567-bf7036f4-beae-4b0d-86f5-70dbe4113d14.jpeg)

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

~ Query 1:

Description: List the different cities of dealerships, count of the number of salespeople in that city, the number of sales that salespeople at that office were sales representatives for, and total sales.

_Code:_

SELECT Dealership.city, COUNT(DISTINCT Salesperson.spID) AS "Number of Salespeople", COUNT(saleNumber) AS "Number of Sales", SUM(saleAmount) AS "Total Sales"
FROM Dealership
JOIN Salesperson ON Dealership.dealershipID = Salesperson.dealershipID
JOIN Sale ON Salesperson.spID = Sale.spID
GROUP BY Dealership.city;

Justification: A manager would like to know which dealerships are most profitable, as well as the relation between sales and number of salespeople per dealership. It will help them with potential dealership expansions and investment in more profitable dealerships.

_Result:_

<img width="280" alt="IMG_5291" src="https://user-images.githubusercontent.com/126897383/229170425-5abef2f7-089f-4930-8b27-88f067f262a4.png">


~ Query 2:  

Description: A query to display the name of the Salesperson, the amount of business they have generated, and the number of sales they have completed in descending order of total sales if they have generated over a certain amount of sales.

_Code:_

SELECT spName, CONCAT("$",ROUND(SUM(saleAmount), 2)) AS "Total Sales", COUNT(saleNumber) AS "Number of Sales"
FROM Salesperson
JOIN Sale ON Salesperson.spID = Sale.spID
GROUP BY spName
HAVING SUM(saleAmount) > 40000k
ORDER BY SUM(saleAmount) DESC;

Justification: A manager can see which of his employees are creating the most business for the company and assigning raises to any salesperson who has generated over $40,000 in sales, as well as a bonus for the top earner.

_Result:_

<img width="240" alt="IMG_4375" src="https://user-images.githubusercontent.com/126897383/229170389-0391fd2c-dc7d-4204-86ba-784497320fc2.png">


~ Query 3:

Description: This is a query that will list the percentage of salesmen who have not completed a sale in any of the dealerships and if they have more than the average amount of experience in the job. 

_Code:_

SELECT CONCAT(ROUND(100*(SELECT COUNT(spName) FROM Salesperson WHERE NOT EXISTS (SELECT * FROM Customer WHERE Customer.spID = Salesperson.spID) AND spExperience > (SELECT avg(spexperience) FROM Salesperson))/COUNT(*),2),'%') AS 'NoSaleWorkers'
FROM Salesperson;

Justification: This will allow for a manager to see what percentage of their workers are not making sales and to disassociate lack of result from the lack of experience. 


_Result:_

<img width="210" alt="IMG_1782" src="https://user-images.githubusercontent.com/126897383/229170307-072a5871-50d9-45b0-934a-852da506b53c.png">


~ Query 4:

Description: This is a query that lists the cities where the dealerships are located, the average payment amount that each dealership makes per sale, the amount of a typical commission for the average sale, and the average profit each dealership makes per sale.

_Code:_

SELECT city, (SUM(paymentAmount)/COUNT(paymentID)) AS "AveragePayment", ((SUM(paymentAmount)/COUNT(Salesperson.spID))*0.25) AS "SalesCommission", ((SUM(paymentAmount)/COUNT(paymentID)*0.75))AS "AverageProfit"
FROM Dealership
JOIN Salesperson ON Dealership.dealershipID = Salesperson.dealershipID
JOIN Customer ON Salesperson.spID = Customer.spID
JOIN Payment ON Customer.customerID = Payment.customerID
GROUP BY city;

Justification: This allows for each dealership to be able to see how much of a payment they are receiving on average and who has the highest out of all of the dealerships, how much commission each employee would typically be getting for a sale, and the overall profit for each sale after taking the employees commission. This can be used to project overage profitability and how much employees make depending on their amount of sales a year.

_Result:_

<img width="250" alt="IMG_0351" src="https://user-images.githubusercontent.com/126897383/229170246-177418ca-6dec-4586-857b-ca7bd4de20d2.png">


~ Query 5:

Description: This is a query that lists the manufacturer name, VIN number, and vehicle year for vehicles that are sedans and modeled between 2014 and 2022 

_Code:_

SELECT Vehicle.VIN, Vehicle.manufacturerName, vehicleYear
FROM Vehicle
JOIN Manufacturer ON Vehicle.manufacturerName=Manufacturer.manufacturerName
WHERE vehicleType = 'sedan' AND vehicleYear BETWEEN 2014 AND 2022;

Justification: This would allow management to keep a catalog of more recently modeled cars for potential customers to browse from if they were interested in purchasing a sedan style vehicle. Since our business specializes in used cars, creating a catalog of more recent manufactured sedans for the customer will push sales of newer cars and thus generate more revenue.

_Result:_

<img width="270" alt="IMG_5150" src="https://user-images.githubusercontent.com/126897383/229170161-c2a9c42b-5d22-4bfe-8ed1-52d5a17a4522.png">


~ Query 6:

Description:  Lists the names of the managers and how many people they supervise

_Code:_ 

SELECT manager.spName, COUNT(*)
FROM Salesperson AS managed
JOIN Salesperson AS manager ON managed.reportsTo = manager.spID
GROUP BY manager.spName;

Justification: A manager needs to keep track of his lower managers and the sales people that they supervise to keep a firm grip on company operations and delegate tasks efficiently down the chain of command. If there is a problem towards the bottom of the company hierarchy, the head manager will know which division manager to contact to reach a solution.

_Result:_

<img width="210" alt="IMG_5966" src="https://user-images.githubusercontent.com/126897383/229170098-84caba8a-45dc-4367-8c64-8aca6d0b2697.png">


~ Query 7:

Description: List the manufacturer name and amount of vehicles they sold if the manufacturing and sale took place in the same country.

_Code:_

SELECT Manufacturer.manufacturerName, COUNT(*)
FROM Manufacturer
JOIN Vehicle ON Manufacturer.manufacturerName = Vehicle.manufacturerName
JOIN Dealership ON Vehicle.dealershipID = Dealership.dealershipID
WHERE hqLocation = country
GROUP BY Manufacturer.manufacturerName;

Justification: Especially in today’s time, economic patriotism has driven consumers to lean towards goods and resources manufactured in their home country. As a manager, you want to see if the country of your dealership has customers more focused on domestic or foreign vehicles.

_Result:_

<img width="220" alt="IMG_2554" src="https://user-images.githubusercontent.com/126897383/229170021-ba8bf30e-1516-44e1-927d-6c6dc59d6c3d.png">


~ Query 8:

Description: List the names, phone numbers, payment type, money owed, and a discounted accounts payable for customers that have 5 or less installments and owe less than the average total amount owed by each customer

_Code:_

SELECT customerName, customerPhone, paymentType, paymentAmount*installments AS 'Money Owed', .9*(paymentAmount*installments) AS 'Discount Price'
FROM Customer
JOIN Payment ON Customer.customerID = Payment.customerID
JOIN Finance ON Payment.financeID = Finance.financeID
WHERE installments < 5 AND paymentAmount*installments < (SELECT AVG(paymentAmount*installments) FROM Payment);

Justification: This query allows the manager to track down specific customers who only have a few pay installments left and do not owe much more money to pay off their finance plan so that they can create more immediate cash flow. If the customer decides to pay upfront, the manager can offer a 10% discounted price to encourage the customer to pay off their debts.

_Result:_

<img width="280" alt="IMG_2240" src="https://user-images.githubusercontent.com/126897383/229169926-51dec781-0e6e-4173-82cf-e88efc42c334.png">


~ Query 9:

Description: List the name, salary, and experience of sales people working in client services and the number of sales they've made 

_Code:_

SELECT spName, spSalary, spExperience, COUNT(saleNumber) AS "Number of Sales"
FROM Salesperson
LEFT JOIN Sale ON Salesperson.spID = Sale.spID
WHERE jobTitle = "Client Service"
GROUP BY spName, spSalary, spExperience;

Justification: This query allows for a manager to evaluate his employees’ performances in the company. The manager with this information can successfully identify which sales people are either exceeding or failing to meet expectations and act accordingly from there.

_Result:_

<img width="230" alt="IMG_2743" src="https://user-images.githubusercontent.com/126897383/229169739-417b22cb-bab0-4c3a-bd18-3af38b60a9b7.png">

~ Query 10:

Description: List the names of manufacturers that have needed service and how many times they’ve needed it.

_Code:_

SELECT Manufacturer.manufacturerName, COUNT(Service.serviceID)
FROM Manufacturer
JOIN Vehicle ON Manufacturer.manufacturerName = Vehicle.manufacturerName
JOIN ServiceRequest ON Vehicle.VIN = ServiceRequest.VIN
JOIN Service ON ServiceRequest.serviceID = Service.serviceID
GROUP BY Manufacturer.manufacturerName;

Justification: This query is important for the manager to assess his choice of vehicle inventory when ordering from different brands. By knowing which vehicles need the most servicing, the manager may decide to cut back on purchasing those vehicles from the manufacturer to improve customer satisfaction and decrease service frequencies. 

_Result:_

<img width="225" alt="IMG_7574" src="https://user-images.githubusercontent.com/126897383/229169532-d75d2312-5d9f-4ea8-9ae2-78d5e45bee27.png">
