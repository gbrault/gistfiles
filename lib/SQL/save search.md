#Purpose
Set the search flow variable `flow.get('sql').search`

#Code
```javascript
flow.get('sql').search=msg.payload;
flow.get('sql').page=0;
return msg;
```
#NODE-RED
`
[{"id":"9ec88988.209e98","type":"function","z":"e81c960d.042c08","name":"save search","func":"flow.get('sql').search=msg.payload;\nflow.get('sql').page=0;\nreturn msg;","outputs":1,"noerr":0,"x":304,"y":328,"wires":[["ac46feba.89eca"]]}]
`
