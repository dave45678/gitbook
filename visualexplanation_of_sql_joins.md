<!--djw
todo: this is a mess. Need local copies of images
-->
###Visual Explanation of SQL Joins
The most critical skill in working with a database is an understanding of the set of records returned by joining tables. It is rare to encounter a database in an enterprise application that consists of just one table. Extracting data from related tables is a critical skill for anyone working with databases.

We'll look at an example with two tables. The table on the left is named ```Table A``` and the table on the right is named ```Table B```.

Each table will contain two columns, ```ID``` and ```Letter``` and 5 records. The data for the left table is A,B,C,D,E the data for the right table is D,E,F,G,H.

There are five ways to combine the data in these two tables. The same concepts apply when there are more tables.
* Inner Join 
* Left Outer Join
* Right Outer Join
* Full Outer Join
* Cross Join

####Inner Join 
Returns all records that are in both ```Table A``` and ```Table B```.
<p><span><img src="http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png" alt="Venn diagram of SQL inner join" /></span></p>

```sql
SELECT * FROM TableA
INNER JOIN TableB
ON TableA.name = TableB.name
```
```
id  name       id   name
--  ----       --   ----
1   Pirate     2    Pirate
3   Ninja      4    Ninja
```
<hr />
####Left Outer Join 
Returns all records in the left table, ```Table A``` and also all the records that are in ```Table B```.
<p><img src="http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702725970c-pi.png" alt="Venn diagram of SQL cartesian join" /></p>
<p>&nbsp;</p>
<pre>SELECT * FROM TableA
FULL OUTER JOIN TableB
ON TableA.name = TableB.name

id    name       id    name
--    ----       --    ----
1     Pirate     2     Pirate
2     Monkey     null  null
3     Ninja      4     Ninja
4     Spaghetti  null  null
5     Frujen     null  null  
null  null       1     Rutabaga
null  null       3     Darth Vader
null  null       5     Han
</pre>

####Full outer join


Returns a set that contains of all the records in ```Table A``` and ```Table B```. Records that both table have in common are also returned once. If ```Table B``` contains a record that is not in ```Table 
B``` then the missing value will contain '```null```.

<p><img src="http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b01287770273e970c-pi.png" alt="Venn diagram of SQL left join" /></p>
<pre>SELECT * FROM TableA
LEFT OUTER JOIN TableB
ON TableA.name = TableB.name

id  name       id    name
--  ----       --    ----
1   Pirate     2     Pirate
2   Monkey     null  null
3   Ninja      4     Ninja
4   Spaghetti  null  null
5   Frujen     null  null
</pre>

####Left outer join  
Returns a set that contains all the records from ```Table A``` along with the matching records (where available) from ```Table B```. Records in the left table only will contain ```null``` for the right side.

<hr />
<p><img src="http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702754970c-pi.png" alt="join-left-outer.png" />&nbsp;</p>
<pre>SELECT * FROM TableA
LEFT OUTER JOIN TableB
ON TableA.name = TableB.name
WHERE TableB.id IS null

id  name       id     name
--  ----       --     ----
2   Monkey     null   null
4   Spaghetti  null   null
5   Frujen     null   null

</pre>
<p>To produce the set of records only in Table A, but not in Table B, we perform the same left outer join, <strong>then exclude the records we don't want from the right side via a where clause</strong>.</p>
<hr />
<p><img src="http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702769970c-pi.png" alt="join-outer.png" /></p>
<pre>SELECT * FROM TableA
FULL OUTER JOIN TableB
ON TableA.name = TableB.name
WHERE TableA.id IS null
OR TableB.id IS null

id    name       id    name
--    ----       --    ----
2     Monkey     null  null
4     Spaghetti  null  null
5     Frujen     null  null
null  null       1     Rutabaga
null  null       3     Darth Vader
null  null       5     Han
</pre>
<p>To produce the set of records unique to Table A and Table B, we perform the same full outer join, then <strong>exclude the records we don't want from both sides via a where clause</strong>.</p>