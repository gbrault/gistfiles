#Purpose
Get the count of rows for the current table, applying whereorall condition (if search !=="")

#Flow variable
##Expected

* qtota
* page_size
* page
* search

##Setting
* total

#Message
##In


##Out

* msg.total
* msg.page_size
* msg.page

#Code
```javascript
context.global.knex.raw(flow.get('sql').qtotal).then(
	    function(resp){
	        // console.log(resp[0].count);
	        msg.total=resp[0].count;
	        flow.get('sql').total=resp[0].count;
	        msg.page_size=flow.get('sql').page_size;
	        msg.page=flow.get('sql').page;
	        node.send(msg);
	    });
return null;
```
#NODE-RED
`
[{"id":"ac46feba.89eca","type":"function","z":"e81c960d.042c08","name":"total, page_size","func":"context.global.knex.raw(flow.get('sql').qtotal).then(\n\t    function(resp){\n\t        // console.log(resp[0].count);\n\t        msg.total=resp[0].count;\n\t        flow.get('sql').total=resp[0].count;\n\t        msg.page_size=flow.get('sql').page_size;\n\t        msg.page=flow.get('sql').page;\n\t        node.send(msg);\n\t    });\nreturn null;","outputs":1,"noerr":0,"x":514,"y":399,"wires":[["c32ae8f2.ab9748","e3cf3c63.fd3cf"]]}]
`
