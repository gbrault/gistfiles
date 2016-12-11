#Purpose

display the total number of rows corresponding to the table and search input

#Context
|Message		    |				        |
| ------------- | ------------- |
| **In**		    | **Out**		    |
|	msg.total	    |             	|

#Code
```html
<small>&nbsp;</small>
<small>{{msg.total}}</small>
```

#NODE-RED
`
[{"id":"e3cf3c63.fd3cf","type":"ui_template","z":"e81c960d.042c08","group":"1b831892.e67a07","name":"total","order":1,"width":"1","height":"1","format":"<small>&nbsp;</small>\n<small>{{msg.total}}</small>","storeOutMessages":true,"fwdInMessages":true,"x":704,"y":460,"wires":[[]]},{"id":"1b831892.e67a07","type":"ui_group","z":"","name":"Table Browser 2","tab":"4782f281.167f9c","order":1,"disp":true,"width":"27"},{"id":"4782f281.167f9c","type":"ui_tab","z":"","name":"SQL 2.0","icon":"dashboard","order":1}]
`
