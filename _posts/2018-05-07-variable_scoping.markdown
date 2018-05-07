---
layout: post
title:      "Variable Scoping."
date:       2018-05-07 18:01:36 +0000
permalink:  variable_scoping
---

Variable Scoping is where a Variable is accessible in a program.

Four types of Variable Scoping:
Global Variable, Local Varialbe, Class Variable and Instance Variable.

Global Variables start with a $ symbol.
Global Variables are accessible from ANYWHERE in the program.
Negative consequences with Global Varaiables can be changed ANYWHERE in the program making troubleshooting difficult.

Local Variables start with lower case letters or an _ symbol.
Local Variables are accessible ONLY from the code they are DEFINED IN. Local Varialbes are DEFINED IN Methods or Loops. Local Variables CANNOT be accessed OUTSIDE of those Methods or Loops.

Class Variables start with @@ symbols.
Class Variables are accessible WITHIN ALL INSTANCES of a CLASS. Similar to GLOBAL VARIABLE except it is available throughout the CLASS. Negative consequence with Class Variable can be changed anywhere in the Class and the Value of ALL the Class Variables are changed.

Instance Variables start with @ symbol.
Instance Variables are accessible to a specific Instance Variable Object. The VALUE of an Instance Variable Object is ONLY that specific Instance Variable Object. Other Instance Variable Objects are NOT changed.


