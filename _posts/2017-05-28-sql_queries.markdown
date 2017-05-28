---
layout: post
title:  SQL Queries
date:   2017-05-28 05:39:02 +0000
---


Be careful about knowing the EXACT NAMES of Tables and columns when using JOIN statements!

SELECT table_name1.column_name1, table_name2.column_name2, table_name3.column_name3 
#SELECT the table_name.column_name of the method you are retrieving data from

FROM table_name1 OR table_name2 OR table_name3
#FROM the table_name1 or 2 or 3

INNER JOIN table_name2 OR table_name3 OR table_name1
#INNER JOIN the other table_name

ON table_name.column_name = another_table_name.column_name
#ON table_name.column_name = another_table_name.column_name
=>Ties the two tables together with their own column_name

GROUP BY table_name.column_name
#if applicable

ORDER BY aggregator(table_name.column_name)
#if applicable

WHERE table_name.column_name = 'condition'

