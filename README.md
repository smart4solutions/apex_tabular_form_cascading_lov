# apex_tabular_form_cascading_lov
Cascading select lists in tabular forms
# Downloading the plugin
You can download the plugin on the [demo site](https://apex.oracle.com/pls/apex/f?p=smart4solutions "Demo site on apex.oracle.com")
# Using the plugin
Inspired by a really old blog by Denes Kubicek, I decided to turn his solution into a usable plugin.
The plugin supports multiple instances on one page as seen here.

1. Create a tabular form
2. Set the relevant items to be a select list
 1. the "parent" or "master" is a regular select list, use SQL, Shared Component, or any other means to populate your select list
 2. the "child" column should have a query that only returns a message" ie:
select '-- please choose --' as d
,      null                  as r
from dual
or it should not return anything
select null as d
,      null as r
from   dual
where  1=0
3. Now you add the plugin (it is a dynamic action plugin)
 1. triggering element:
This is the "master", you need to enter the NAME attribute of the item, find it by using your browsers' inspector
 2. row key:
This is the element that holds the primary key of your tabular form. It is needed to be able to select the child's value. Find it by searching for the name attribute for the (hidden) element that holds the rows' key.
The plugin can only handle singular primary keys, use rowid if needed
 3. child element:
This is the "child", you need to enter the NAME attribute of the item, find it by using your browsers' inspector
 4. nothing selected text:
Enter the text that must be displayed when nothing is chosen. (ie: "-- please make a choice --")
 5. query:
Enter the query responsible for populating the child element. The query should return two columns: d and r (for display-value and return-value) and contain a where clause that refers to the parent elements value. That parent elements value should be preceded by a colon ("bind" parameter), as shown in the example:
select ename d
,      empno r 
from   emp 
where  deptno = :deptno
order by 1
 6. Child query:
Enter the query that gets the current child elements value from the database. The query should return a single row and a single column and should have a bind variable in the where clause.
select empno
from   emplog 
where  id = :l_key_val
4. That should do the trick. Run your page.
