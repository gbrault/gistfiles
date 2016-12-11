#Purpose
Drop-down select human interface to pick the database's table

This component is the genuine node-red-dashboard dropdown component to which "sql functions+ List of Table" provides msg.options and 
set the first table as the selected option.

#NODE RED
`
[{"id":"2185d632.97e08a","type":"ui_dropdown","z":"e81c960d.042c08","name":"Table choice","label":"","group":"1b831892.e67a07","order":2,"width":"5","height":"1","passthru":true,"options":[{"label":"","value":"","type":"str"}],"payload":"","topic":"","x":334,"y":517,"wires":[["ab5a26ee.990b28"]]},{"id":"1b831892.e67a07","type":"ui_group","z":"","name":"Table Browser 2","tab":"4782f281.167f9c","order":1,"disp":true,"width":"27"},{"id":"4782f281.167f9c","type":"ui_tab","z":"","name":"SQL 2.0","icon":"dashboard","order":1}]
`
