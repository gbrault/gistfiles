#Purpose
set msg.payload with qpage request (which use whereorall)

#Context
| Flow variable |				        |Message		    |				                      |
| ------------- | ------------- | ------------- | --------------------------- |
| **Expected**	| **Setting** 	| **In**		    | **Out**		                  |
| table   		  | page     		  |	msg.page      | msg.payload := sql request 	|
| search        | page_size     | msg.page_size |                             |

#SQL Request
```javascript
select rowid, * from  "+flow.get('sql').table+flow.get('sql').whereorall+" order by rowid limit "+
                        flow.get('sql').page_size+" offset "+flow.get('sql').page_size*(flow.get('page')-1)
```
requesting ordering with rowid is requested to get table rows creation order (this is database backend dependent) 

#Code
```javascript
flow.get('sql').page=msg.page;
flow.get('sql').page_size=msg.page_size;
msg.payload=flow.get('sql').qpage;
return msg;
```
#NODE-RED
`
[{"id":"81998703.eea268","type":"function","z":"e81c960d.042c08","name":"sql page request","func":"flow.get('sql').page=msg.page;\nflow.get('sql').page_size=msg.page_size;\nmsg.payload=flow.get('sql').qpage;\nreturn msg;","outputs":1,"noerr":0,"x":474,"y":271,"wires":[["226b6388.c3db4c","b680954.c1ef968"]]}]
`
