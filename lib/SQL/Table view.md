#Purpose

Dynamic display as a grid of the sql table, highlighting the search text.

#Context
|Message		    |				        |
| ------------- | ------------- |
| **In**		    | **Out**		    |
|	msg.payload	  |               |

msg.payload contains the json encoded table values as an array of objects, which attributes are sql table names.
It use the first array.

#Code
```html
<table>
<tr>
  <th ng-repeat="(key,value) in table[0]">{{key}}</th>
</tr>
<tbody ng-repeat="row in table">
<tr ng-if="$even">
  <td ng-repeat="(key,value) in row" ng-bind-html="decorate(value)"></td>
</tr>
<tr ng-if="$odd">
  <td style="background-color:#f1f1f1" ng-repeat="(key,value) in row" ng-bind-html="decorate(value)"></td>
</tr>
</tbody> 
</table>
<style>
table, td  {
  border: 1px solid grey;
  border-collapse: collapse;
  padding: 5px;
}
.highlighted { background: yellow }
</style>
<script>
(function(scope) {
    // debugger;
    scope.table=[];
    scope.search="";
    scope.$watch('msg', function (newValue, oldValue, scope) {
            scope.table=JSON.parse(scope.msg.payload)[0];
            scope.search = scope.msg.search;
    });
    scope.decorate = function(value){
        // debugger;
        var tmp="";
        if ((scope.search)  && (($.type(value) === "string")))
            tmp = value.replace(  new RegExp('('+scope.search+')', 'gi'),
                                    '<span class="highlighted">$1</span>');
        else
            tmp = value;
        if (tmp.length==0) return value; else return tmp;
    };
})(scope);    
</script>
```

#NODE-RED
`
[{"id":"ebe312d9.60d79","type":"ui_template","z":"e81c960d.042c08","group":"1b831892.e67a07","name":"Table View","order":4,"width":"0","height":"0","format":"<table>\n<tr>\n  <th ng-repeat=\"(key,value) in table[0]\">{{key}}</th>\n</tr>\n<tbody ng-repeat=\"row in table\">\n<tr ng-if=\"$even\">\n  <td ng-repeat=\"(key,value) in row\" ng-bind-html=\"decorate(value)\"></td>\n</tr>\n<tr ng-if=\"$odd\">\n  <td style=\"background-color:#f1f1f1\" ng-repeat=\"(key,value) in row\" ng-bind-html=\"decorate(value)\"></td>\n</tr>\n</tbody> \n</table>\n<style>\ntable, td  {\n  border: 1px solid grey;\n  border-collapse: collapse;\n  padding: 5px;\n}\n.highlighted { background: yellow }\n</style>\n<script>\n(function(scope) {\n    // debugger;\n    scope.table=[];\n    scope.search=\"\";\n    scope.$watch('msg', function (newValue, oldValue, scope) {\n            scope.table=JSON.parse(scope.msg.payload)[0];\n            scope.search = scope.msg.search;\n    });\n    scope.decorate = function(value){\n        // debugger;\n        var tmp=\"\";\n        if ((scope.search)  && (($.type(value) === \"string\")))\n            tmp = value.replace(  new RegExp('('+scope.search+')', 'gi'),\n                                    '<span class=\"highlighted\">$1</span>');\n        else\n            tmp = value;\n        if (tmp.length==0) return value; else return tmp;\n    };\n})(scope);    \n</script>","storeOutMessages":true,"fwdInMessages":true,"x":755,"y":207,"wires":[[]]},{"id":"1b831892.e67a07","type":"ui_group","z":"","name":"Table Browser 2","tab":"4782f281.167f9c","order":1,"disp":true,"width":"27"},{"id":"4782f281.167f9c","type":"ui_tab","z":"","name":"SQL 2.0","icon":"dashboard","order":1}]
`
