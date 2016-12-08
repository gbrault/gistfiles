#Purpose

Install, flow and nodes documentation to create an sqlite database table browser in node-red

#Result

1. Select a table from Northwind database
2. View selected table page (of 5 rows: fixed by design)
3. Select database page to view

![alt tag](https://raw.githubusercontent.com/gbrault/gistfiles/master/knex/knex.png)

#Components

They are 4 sections in the following flow

* A web service which allows direct access to Northwind data
 * nodes: 1,2,3,7,4,5
* A "Table choice" section: to select one of the tables of Northwind
 * nodes: 17, 18, 19, 16
* A "Table view" section: to show a page of the selected table
 * nodes: 6,9,7,4,8,10,11
* A "paging controller" section: to control table page browsing
 * nodes: 12, 13, 14, 15, 10

![alt tag](https://raw.githubusercontent.com/gbrault/gistfiles/39b80f3357924bf288ed1c9890c93ca0d0407c54/knex/knex%20flow.png)

##A web service which allows direct access to Northwind data
 nodes: 1,2,3,7,4,5
 
json response of any sql query from the url
* **Query**
```html 
http://localhost:1880/sql?sql=pragma table_info('customer') 
```
* **Response**
```javascript
[[{"cid":0,"name":"IdCustomer","type":"text","notnull":0,"dflt_value":null,"pk":1},
{"cid":1,"name":"CompanyName","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":2,"name":"ContactName","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":3,"name":"ContactTitle","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":4,"name":"Address","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":5,"name":"City","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":6,"name":"Region","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":7,"name":"PostalCode","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":8,"name":"Country","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":9,"name":"Phone","type":"text","notnull":0,"dflt_value":null,"pk":0},
{"cid":10,"name":"Fax","type":"text","notnull":0,"dflt_value":null,"pk":0}]]
```

* **Operation**

1. an http request is processed by node 1: "sql webservice" (http in)
2. the "prepare sql" function node set the msg fields from node 1 to comply with node 7 logic
3. this is a debug node to trace payload value during development
4. the "http?" function node check if the request comes from webservice or internal request
5. this block is the http response of block 1

 
##A "Table choice" section: to select one of the tables of Northwind
 nodes: 17, 18, 19, 16
 
(17) This is an injection node to trigger table gathering at startup<br/>
(18) Logic to request and build the table list of Northwind<br/>
(16) A 'drop-down' node-red-dashboard widget to display and control the table list<br/>
(19) A debug msg watch<br/>
 
##A "Table view" section: to show a page of the selected table
 nodes: 6,9,7,4,8,10,11
 
##A "paging controller" section: to control table page browsing
 nodes: 12, 13, 14, 15, 10
 


#Install
