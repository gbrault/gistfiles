A set of variables are availble at the flow level. They are accessible using ```flow.get('sql').'variable symbole'```
* table = table name
* qtotal = sql query to get table count (use whereorall)
* total = row count of table
* page_size = page size parameter
* page = page number starts at 1
* qpage = sql query to get a table page of page_size
* search = the sentence to search in the current table
* columns = an array of structures for each column of the table
* whereorall = where statement with all the columns of the table

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
