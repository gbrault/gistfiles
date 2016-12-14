```
strict digraph CT {
 rankdir=LR
 splines=ortho
 node [style=rounded]
 node [shape=box]

begin [shape=doublecircle]
point0 [shape=point] 
point1 [shape=point]
point2 [label="2" shape=circle]
point3 [label="2" shape=circle]
point4  [label="3" shape=circle]
point5  [label="3" shape=circle]

begin->point3 [style="invis"]
point3->point5 [style="invis"]
{rank=same;begin;point3;point5}

point3->table_name
point3->{schema_name [shape=note]}-> {circle1 [label="." shape=circle] }->{table_name[shape=note]}


TABLE ->IF->NOT->EXISTS->point1
TABLE -> point1
point1->point2
{
 begin -> CREATE -> { point0 TEMP TEMPORARY } ->TABLE
} 

table_name->point4
point5->{paro[label="("]}->{back [shape=point]}->{coldef [label="column-def" shape=box style=""]}->{vir [label="," shape=circle]}->back
coldef->{point6 [shape=point]}->{parc[label=")" shape=circle]}
point6->{vir1[label="," shape=circle]}->{tabconst [label="table-constraint" shape=box style=""]}->point6
{rank=same;coldef;vir}
{rank=same;vir1;point6}
point5->AS->{selstat [label="select-statement" shape=box style=""]}->{point7 [shape=doublecircle label="end"]}
parc->WITHOUT->ROWID->point7
parc->point7


}
```
