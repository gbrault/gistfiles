#Purpose

The role of this human interface is to control table browsing.

#Context
|Message		|				|
| ------------- | ------------- |
| **In**		| **Out**		|
|	msg.total	| msg.page    	|
| msg.page_size | msg.page_size |
| msg.page      |               |


#Code
```html
<div paging page="page" page-size="page_size" total="total" paging-action="paging(page, pageSize, total)">
</div>
<script>
(function(scope) {
    scope.total=0;
    scope.page_size=0;
    scope.page=0;
    scope.$watch('msg', function (newValue, oldValue, scope) {
        // debugger;
        if(scope.msg!==undefined){
            scope.total=scope.msg.total;
            scope.page_size=scope.msg.page_size;
            scope.page=scope.msg.page;
        }
            
    });
    scope.paging=function(page,pageSize,total){
        // debugger;
        scope.msg.page=page;
        scope.msg.page_size=pageSize;
        scope.send(scope.msg);
    };
})(scope); 
</script>
```

#NODE-RED
`
[{"id":"c32ae8f2.ab9748","type":"ui_template","z":"e81c960d.042c08","group":"1b831892.e67a07","name":"paging controller","order":5,"width":0,"height":0,"format":"<div paging page=\"page\" page-size=\"page_size\" total=\"total\" paging-action=\"paging(page, pageSize, total)\">\n</div>\n<script>\n(function(scope) {\n    scope.total=0;\n    scope.page_size=0;\n    scope.page=0;\n    scope.$watch('msg', function (newValue, oldValue, scope) {\n        // debugger;\n        if(scope.msg!==undefined){\n            scope.total=scope.msg.total;\n            scope.page_size=scope.msg.page_size;\n            scope.page=scope.msg.page;\n        }\n            \n    });\n    scope.paging=function(page,pageSize,total){\n        // debugger;\n        scope.msg.page=page;\n        scope.msg.page_size=pageSize;\n        scope.send(scope.msg);\n    };\n})(scope); \n</script>","storeOutMessages":true,"fwdInMessages":true,"x":735,"y":399,"wires":[["bfa92b69.0fb078","81998703.eea268"]]},{"id":"1b831892.e67a07","type":"ui_group","z":"","name":"Table Browser 2","tab":"4782f281.167f9c","order":1,"disp":true,"width":"27"},{"id":"4782f281.167f9c","type":"ui_tab","z":"","name":"SQL 2.0","icon":"dashboard","order":1}]
`
