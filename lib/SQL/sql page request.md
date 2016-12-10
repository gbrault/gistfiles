#Purpose
set msg.payload with qpage request (which use whereorall)

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
