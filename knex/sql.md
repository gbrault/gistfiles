#Purpose

Install, flow and nodes documentation to browse an sqlite database in node-red

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
 * 17, 18, 19, 16
* A "Table view" section: to show a page of the selected table
 * nodes: 6,9,7,4,8,10,11
* A "paging controller" section: to control table page browsing
 * 12, 13, 14, 15, 10

![alt tag](https://raw.githubusercontent.com/gbrault/gistfiles/39b80f3357924bf288ed1c9890c93ca0d0407c54/knex/knex%20flow.png)

from the url: i.e. ``` <host>/sql?sql=pragma table_info('customer') ```

#Install
