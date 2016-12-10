#Purpose
this function node fetch the columns structure when the table changes (table expected in msg.payload)

#Columns structure
an array of object
```javascript
{"cid":0,                   /* column order identifier  */
 "name":"IdOrderDetail",    /* column symbol            */
 "type":"text",             /* type of data: text, integer, double... */
 "notnull":0,               /* can be null?             */
 "dflt_value":null,         /* default value            */
 "pk":1}                    /* is it part of the primary key */
 ```
#Message
##In
* msg.payload = table

##Out
* msg in

#CODE
```javascript
flow.get('sql').table=msg.payload;
/* get a list of associated columns  backend dependent*/
context.global.knex.raw('pragma table_info('+flow.get('sql').table+')').then(
    function(resp){
        flow.get('sql').columns=resp;
        // console.log(resp);
        node.send(msg);
    }
);
return null;
```

#NODE RED
`[{"id":"ab5a26ee.990b28","type":"function","z":"e81c960d.042c08","name":"get columns","func":"flow.get('sql').table=msg.payload;\n/* get a list of associated columns  backend dependent*/\ncontext.global.knex.raw('pragma table_info('+flow.get('sql').table+')').then(\n    function(resp){\n        flow.get('sql').columns=resp;\n        // console.log(resp);\n        node.send(msg);\n    }\n);\nreturn null;","outputs":1,"noerr":0,"x":428,"y":460,"wires":[["ac46feba.89eca"]]}]
`
