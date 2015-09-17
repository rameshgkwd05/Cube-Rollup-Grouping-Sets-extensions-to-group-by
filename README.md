# Cube-Rollup-Grouping-Sets-extensions-to-group-by
Cube/Rollup/Grouping Sets extensions to group by in PostgresSQL-9.3.5.

#Project Description

OLAP typically aggregate data across multiple dimensions. The GROUP BY operator produce zero-dimensional or one-dimensional answers.  Applications need the N-dimensional generalization of these operators. So there are operation like CUBE, ROLLUP,GROUPING SETS which work well with multi-dimensional data are used in DBMS.

#Grouping sets
GROUPING SETS operator helps to get multiple GROUP BY results using a single statement. Grouping set applies join only on the specified combinations in grouping set clause. 
For example, A = name, place, (name, place)
Grouping set equivalent = name, place, (name, place)

#Cube
CUBE operator computes union of GROUP By’s on every subset of the specified attributes.
For example, A = name, place
Cube equivalent = (), name, place, (name, place)
For cube we take power set of the grouping set list.

#Rollup
ROLLUP operator generate union on every prefix of specified list of attributes and also help to move from fine-granularity to coarser granularity(by means of aggregation).
For example, A = name, place
Roll up equivalent = (), name, (name, place)
For roll up we take the prefixes of the grouping set list.

#Overview of Implementation:

The following figure outlines the flow of the program. Query is tokenized and given to
parser (gram.y). The query is then parsed to generate a parse tree. We intercept this
parse tree and modify the tree to match the parse tree of grouping set.
![overview]

#Grouping set implementation
For grouping sets, we iterate over the list of group by columns present in the group by list. For each entry in list we create a select query parse tree with group by that column.All the columns which are not present in group by list and are not aggregate functions are converted to null. We also cast the target columns to string while displaying the output.
![GroupingSetImpl]

#Instructions for installing  patch:

1) Assume that the original PGSQL code to be patched is under:

  	<orig>/postgresql-9.3.5_orig

2) Copy the patch file "patch_cuberollup_rar.txt" to <orig> directory

cp patch_cuberollup_rar.txt <orig>/

3) Change to the "orig" directory
	
cd <orig>

4) Run the "patch" command as follows:

patch -p0 < patch_cuberollup_rar.txt

5) Now build the postgres in usual way and fire valid grouping sets queries.
 
instructions for building postgres are available at:
http://www.cse.iitb.ac.in/dbms/Data/Courses/CS631/PostgreSQL-Resources/pgsql_demo.txt
