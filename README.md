# SQL Mystery
## Problem Statement
On October 23rd, 2021 Kehinde Wiley's, infamous painting, "Napoleon Leading the Army Over the Alps" was stolen from the New York City Metropolitan Museum of Art. 
The only evidence that the authorities have is a digital evidence file containing flight, bank, cell phone and other related data.
Because of a lack of expertise, the authorities have been unable to analyze the file to make any headway in the case. 
<br><br>Now, this is where you come in. YOU are the last and only resort for cracking the case. 
<br><br>Use your SQL data skills to do 1. analyze the data. 2. gather evidence. 3. find the thieves. 4. find that painting.

### Solution:

<b>Query # 1: Do we have anyone in our database who resides in NYC?</b>
~~~~sql
SELECT
id AS [Customer Number],
firstName AS [First Name],
LastName AS [Last Name],
city AS City,
age AS Age
FROM customer_details
WHERE city = 'New York City'
;
~~~~
| Customer Number |First Name |Last Name  |City			|Age|
|-----------------|-----------|-----------|-------------|---|
| 32			  |Barry	  |Allen	  |New York City|35 |
| 38			  |Opra		  |Baker	  |New York City|43 |
<hr>

<b>Query # 2: who flew to New York City before the date of the crime, October 23rd, 2021?</b>
~~~~sql
SELECT
customer_id AS [Customer Number],
CusFName AS [First Name],
CusLName AS [Last Name],
start_city AS [Departure City],
dest_city AS [Arrival City],
flightDate AS [Flight Date]
FROM flight_details
WHERE dest_city = 'New York City'
AND flightDate <= '2021-10-23'
;
~~~~
|Customer Number |First Name	|Last Name	|Departure City		|Arrival City	|Flight Date		|
|----------------|--------------|-----------|-------------------|---------------|-------------------|
|100			 |Bruce			|Fisher		|Puerto Princesa	|New York City	|2021-10-20 08:00:00|
|105			 |Aleana		|Jordan		|Ouagadougou		|New York City	|2021-10-21 08:00:00|
|106			 |Brenda		|Reynolds	|Karatsu			|New York City	|2021-10-21 08:00:00|
<hr>

<b>Query # 3: Identify if anyone flew from New York City after the day the painting was stolen?</b>
~~~~sql
SELECT
customer_id AS [Customer Number],
CusFName AS [First Name],
CusLName AS [Last Name],
start_city AS [Departure City],
dest_city AS [Arrival City],
flightDate AS [Flight Date]
FROM flight_details
WHERE start_city = 'New York City'
AND flightDate >= '2021-10-23'
;
~~~~
|Customer Number |First Name	|Last Name	|Departure City	|Arrival City		|Flight Date		 |
|----------------|--------------|-----------|---------------|-------------------|--------------------|
|100			 |Bruce			|Fisher		|New York City	|Puerto Princesa	|2021-10-24 08:00:00 |
|105			 |Aleana		|Jordan		|New York City	|Moskow				|2021-10-25 08:00:00 |
|106			 |Brenda		|Reynolds	|New York City	|Karatsu			|2021-10-24 08:00:00 |
|141			 |Hajrah		|Burns		|New York City	|Tianjin			|2021-10-31 00:00:00 |
<hr>

<p>Above analysis tells us that <b>Bruce Fisher, Aleana Jordan and Brenda Reynolds</b> all traveled to New York City before the date of the crime and they all left after the date of the crime. 
  This means that they were all physically present at the time and place that the crime took place. Then, they all left shortly after. 
  Also <b>Barry Allen and Opra Baker</b> are resident of NYC who were physically present at the time and place that the crime took place.
Because of this, we are going to take note of these folks as persons of interest. </p>
<hr>

<b>Query # 4: Let's check how many messages were sent between October 20th and October 25th, which is a couple days before and after the date of our crime on October 23rd.
Also filter by people we have identified as persons of interest.</b>

~~~~sql
SELECT 
sender_id AS [Sender ID],
SenderFName AS [Sender First Name],
SenderLName AS [Sender Last Name],
receiver_id AS [Receiver ID],
RecieverFName AS [Receiver First Name],
RecieverLName AS [Receiver Last Name],
message AS [Text Message],
sent AS [Date Sent]
FROM text_messages
WHERE sent BETWEEN '2021-10-20' AND '2021-10-25'
AND sender_id IN (32,38,100,105,106)
ORDER BY sent
;
~~~~

![image](https://github.com/manaswipatil/Data-Analysis-Investigate-with-SQL/assets/50437663/339b0f8f-76ac-4d62-aa4a-a6226febfb2e)
<hr>

<p>From above query result of text exchanges between suspects, We can now conclude based on our manual analysis, 
  that <b>Bruce and Brenda</b> are indeed the thieves responsible for the theft of Mr. Kehinde Wiley's infamous painting. </p>
