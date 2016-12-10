#Purpose
execute a series of sql requests (separated with ;) and return an array of responses (one foreach sql input statement)

#Context

| Flow variable |	        |Message		             |				                 	|
| ------------- | ------------- | ---------------------------------- | ------------------------------------------------ |
| **Expected**	| **Setting** 	| **In**		             | **Out**		                         	|
|               |  		| msg.payload	:= sql statements    |  msg.payload := [jsonresponse1,jsonresponse2] 	|

##Use of flow variable
this node manage statements, count and exec flow variables
* statements: split of incoming msg.payload (separated by ;)
* count: which is the next active statement
* exec: function that is recursively called till all statements executed

#Code
```javascript
var statements = msg.payload.split(";");
flow.set('responses',[]);
flow.set('statements',statements);
flow.set('count',0);
flow.set('exec', function(){
    context.global.knex.raw(flow.get('statements')[flow.get('count')]).then(
	    function(resp){
	        var responses = flow.get('responses');
	        responses.push(resp);
	        flow.set('count',flow.get('count')+1);
	        if(flow.get('count')>=flow.get('statements').length){
	            msg.payload = JSON.stringify(responses);
	            msg.headers = {
                     'Content-type' : 'application/json'
                };
	            node.send(msg);
	        } else {
	            flow.get('exec')();
	        }
	    }
    );
} );
flow.get('exec')();
return null;
```

#NODE-RED
`
[{"id":"226b6388.c3db4c","type":"function","z":"e81c960d.042c08","name":"sql exec","func":"var statements = msg.payload.split(\";\");\nflow.set('responses',[]);\nflow.set('statements',statements);\nflow.set('count',0);\nflow.set('exec', function(){\n    context.global.knex.raw(flow.get('statements')[flow.get('count')]).then(\n\t    function(resp){\n\t        var responses = flow.get('responses');\n\t        responses.push(resp);\n\t        flow.set('count',flow.get('count')+1);\n\t        if(flow.get('count')>=flow.get('statements').length){\n\t            msg.payload = JSON.stringify(responses);\n\t            msg.headers = {\n                     'Content-type' : 'application/json'\n                };\n\t            node.send(msg);\n\t        } else {\n\t            flow.get('exec')();\n\t        }\n\t    }\n    );\n} );\nflow.get('exec')();\nreturn null;","outputs":1,"noerr":0,"x":469,"y":159,"wires":[["ce9475a6.52c3d8","c6d5f352.65145"]]}]
`
