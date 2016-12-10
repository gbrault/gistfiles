A set of variables are availble at the flow level. They are accessible using ```flow.get('sql').'variable symbole'```
* table = table name
* qtotal = sql query to get table count (use whereorall)
* total = row count of table
* page_size = page size parameter
* page = page number starts at 1
* qpage = sql query to get a table page of page_size  (use whereorall)
* search = the sentence to search in the current table
* columns = an array of structures for each column of the table
* whereorall = where statement with all the columns of the table

Code
```javascript
/* create the sql getter and setters */
flow.set('sql',{
    get table() { return flow.get('table');},
    set table(t) { flow.set('table',"'"+t+"'");},
    get qtotal(){ return 'select Count(*) as count from '+flow.get('table')+ flow.get('sql').whereorall;},
    get total() { return flow.get('total');},
    set total(t) {flow.set('total',t);},
    get page_size() {return flow.get('page_size');},
    set page_size(s) {flow.set('page_size',s);}, 
    get page() { return flow.get('page');},
    set page(p) {flow.set('page',p);},
    get qpage(){ return "select rowid, * from  "+
                        flow.get('table')+flow.get('sql').whereorall+" order by rowid limit "+
                        flow.get('page_size')+" offset "+flow.get('page_size')*(flow.get('page')-1);},
    get search() {return flow.get('search');},
    set search(s) {flow.set('search',s);},
    set columns(c) {flow.set('columns',c);},
    get columns() { return flow.get('columns');},
    get whereorall() {
        var where = "";
        var search = flow.get('search');
        if((search!==undefined)&&(search!==null)&&(search.length>0)){
            var columns = flow.get('columns');
            if((columns!==undefined)&&(columns!==null)&&(columns.length>0)){
                where = " where ";
                for(var i=0; i< columns.length; i++){
                    where = where + columns[i].name+ " like '%"+flow.get('search')+"%'"; 
                    if (i<columns.length-1){
                        where = where + " or ";
                    }
                }
            }
        }
        return where;
    }
});
/* set the page_size */
flow.get('sql').page_size=5;
flow.get('sql').page=1;
/* get a list of table backend dependent */
context.global.knex.raw('select * from sqlite_master').then(
	    function(resp){
	        msg.options=[];
	        for(var i=0; i<resp.length;i++){
	            if(resp[i].type==='table')
	                msg.options.push(resp[i].name);
	        }
	        msg.options=msg.options.sort();
	        msg.payload=msg.options[0];
	        flow.get('sql').table=msg.options[0];
	        node.send(msg);
	    }
);
return null;
```
NODE-RED
```
[{"id":"c541ae3e.28cb9","type":"function","z":"e81c960d.042c08","name":"sql functions + List of tables","func":"flow.set('sql',{\n    get table() { return flow.get('table');},\n    set table(t) { flow.set('table',\"'\"+t+\"'\");},\n    get qtotal(){ return 'select Count(*) as count from '+flow.get('table')+ flow.get('sql').whereorall;},\n    get total() { return flow.get('total');},\n    set total(t) {flow.set('total',t);},\n    get page_size() {return flow.get('page_size');},\n    set page_size(s) {flow.set('page_size',s);}, \n    get page() { return flow.get('page');},\n    set page(p) {flow.set('page',p);},\n    get qpage(){ return \"select rowid, * from  \"+\n                        flow.get('table')+flow.get('sql').whereorall+\" order by rowid limit \"+\n                        flow.get('page_size')+\" offset \"+flow.get('page_size')*(flow.get('page')-1);},\n    get search() {return flow.get('search');},\n    set search(s) {flow.set('search',s);},\n    set columns(c) {flow.set('columns',c);},\n    get columns() { return flow.get('columns');},\n    get whereorall() {\n        var where = \"\";\n        var search = flow.get('search');\n        if((search!==undefined)&&(search!==null)&&(search.length>0)){\n            var columns = flow.get('columns');\n            if((columns!==undefined)&&(columns!==null)&&(columns.length>0)){\n                where = \" where \";\n                for(var i=0; i< columns.length; i++){\n                    where = where + columns[i].name+ \" like '%\"+flow.get('search')+\"%'\"; \n                    if (i<columns.length-1){\n                        where = where + \" or \";\n                    }\n                }\n            }\n        }\n        return where;\n    }\n});\n/* set the page_size */\nflow.get('sql').page_size=5;\nflow.get('sql').page=1;\n/* get a list of table backend dependent */\ncontext.global.knex.raw('select * from sqlite_master').then(\n\t    function(resp){\n\t        msg.options=[];\n\t        for(var i=0; i<resp.length;i++){\n\t            if(resp[i].type==='table')\n\t                msg.options.push(resp[i].name);\n\t        }\n\t        msg.options=msg.options.sort();\n\t        msg.payload=msg.options[0];\n\t        flow.get('sql').table=msg.options[0];\n\t        node.send(msg);\n\t    }\n);\nreturn null;","outputs":1,"noerr":0,"x":188,"y":584,"wires":[["2185d632.97e08a","138b457a.a7a34b"]]}]
```
