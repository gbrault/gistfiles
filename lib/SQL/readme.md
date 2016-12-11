#SQL Node-Red, nodes and flows SQLITE Library

##knex 2.0: Browse and Search any table in an SQLITE Database

* function nodes
 * sql functions + List of Tables [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/sql%20functions%20-%20List%20of%20Tables.md)
 * get columns [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/get%20columns.md)
 * total,page_size [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/total%2Cpage_size.md)
 * save search [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/save%20search.md)
 * sql page request [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/sql%20page%20request.md)
 * sql exec [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/sql%20exec.md)
* ui nodes (node-red-dashboard)
 * Table choice [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/Table%20choice.md)
 * paging controller [see](https://github.com/gbrault/gistfiles/blob/master/lib/SQL/paging%20controller.md)
 * total [see]()
 * Table view [see]()
 
##Flow example

###Node Red

| N.  | Name              | Node Type            | Comment                                                                 |
| --- | ----------------- | -------------------- | ----------------------------------------------------------------------- |
|  01 | sql webservice    | http request server  | sql webservice http://host:port/sql?sql=request                         |
|  02 | prepare sql       | function             | process http request variables to prepare sql request                   |
|  03 | msg.payload       | debug                | debugging                                                               |
|  04 | http?             | function             | check if request comes from webservice or internal                      |
|  05 | http              | http response        | response from http request                                              |
|  06 | supplier          | inject               | inject supplier sql request for testing purpose                         |
|  07 | sql exec          | function             | execute sql request using knex                                          |
|  08 | msg.payload       | debug                | debugging                                                               |
|  09 | customer          | inject               | inject customer sql request for testing purpose                         |
|  10 | sql page request  | function             | build the sql page request based upon global flow variables             |
|  11 | Table View        | dashboard template   | build angular.js table view user interface                              |
|  12 | search            | dashboard text input | search input user interface                                             |



![alt_tag](https://raw.githubusercontent.com/gbrault/gistfiles/master/lib/SQL/Sqlite%20Table%20Browse%20and%20Search.png)
