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

(1) an http request is processed by node 1: "sql webservice" (http in)<br/>
(2) the "prepare sql" function node set the msg fields from node 1 to comply with node 7 logic<br/>
(3) this is a debug node to trace payload value during development<br/>
(4) the "http?" function node check if the request comes from webservice or internal request<br/>
(5) this block is the http response of block 1<br/>

 
##A "Table choice" section: to select one of the tables of Northwind
 nodes: 17, 18, 19, 16
 
(17) This is an injection node to trigger table gathering at startup<br/>
      ``` context.global.knex.raw('select * from sqlite_master')```<br/>
(18) Logic to request and build the table list of Northwind<br/>
(16) A 'drop-down' node-red-dashboard widget to display and control the table list<br/>
(19) A debug msg watch<br/>
 
##A "Table view" section: to show a page of the selected table
 nodes: 6,9,7,4,8,10,11
 
(6) and (9) test to inject supplier and customer table select page 0 (limit 5 offset 0)* for testing purpose<br/>
(7) the sql exec logic getting sql queries in input and returning one array per query. Queries are separated by semi-column (;)<br/>
(4) test if webservice or internal request<br/>
(8) debug node to watch payload out of sql exec<br/>
(10) sql page request (used by the paging controller)<br/>
(11) gui result as a node-red-dashboard template node<br/>

(*) according to sqlite documentation and this might be true for other database backend as well, ordering by rowid is necessary to get the table rows sorted according to the creation order
 
##A "paging controller" section: to control table page browsing
 nodes: 12, 13, 14, 15, 10
 
(12) an injection node for testing purpose<br/>
(13) the function node calculating the acual table size and setting the page size =5<br/>
      ``` context.global.knex.raw('select Count(*) as count from '+msg.table)```<br/>
(14) the paging component: a node-red-dashboard template node<br/>
(15) msg debug node<br/>
 
#Detailling "Table View" and "Paging Controller"

##Table View

```html
<table>
<tr>
  <th ng-repeat="(key,value) in table[0]">{{key}}</th>
</tr>
<tbody ng-repeat="row in table">
<tr ng-if="$even">
  <td ng-repeat="(key,value) in row">{{value}}</td>
</tr>
<tr ng-if="$odd">
  <td style="background-color:#f1f1f1" ng-repeat="(key,value) in row">{{value}}</td>
</tr>
</tbody> 
</table>
<style>
table, td  {
  border: 1px solid grey;
  border-collapse: collapse;
  padding: 5px;
}
</style>
<script>
(function(scope) {
    // debugger;
    scope.table=[];
    scope.$watch('msg', function (newValue, oldValue, scope) {
            scope.table=JSON.parse(scope.msg.payload)[0];
    });
})(scope);    
</script>
```

#Install
