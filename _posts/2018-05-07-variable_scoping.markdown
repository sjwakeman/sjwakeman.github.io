---
layout: post
title:      "Variable Scoping."
date:       2018-05-07 14:01:37 -0400
permalink:  variable_scoping
---

Variable Scoping is where the Variable is accessible in the program code.

There are four types of Variable Scoping:
Global Variable, Local Variable, Class Variable and Instance Variable.

Global Variables -  $variable_name
Start with a $ symbol.  
Global Variables are accessible from ANYWHERE in the program code.
Negative consequences with Global Varaiables can be changed ANYWHERE in the program making troubleshooting difficult. Meaning the VALUE for the Global Variable is where the Global Variable was last executed in the program code.

Local Variables -  variable_name OR  _variable_name
Start with lower case letters or an _ symbol.  
Local Variables are accessible ONLY WHERE the code APPEARS. Local Varialbes APPEAR IN Methods or Loops. Local Variables CANNOT be accessed OUTSIDE of those Methods or Loops. Meaning the VALUE for the Local Variable 

Class Variables -  @@variable_name
Start with @@ symbols.  
Class Variables are accessible WITHIN ALL INSTANCES of a CLASS. Similar to GLOBAL VARIABLE except it is available throughout the CLASS. Negative consequence with Class Variable can be changed anywhere in the Class and the Value of ALL the Class Variables are changed each time it is executed in the program code.

Instance Variables -  @variable_name
Start with @ symbol. 
Instance Variables are accessible to a SPECIFIC Instance Variable Object. The VALUE of an Instance Variable Object is ONLY that SPECIFIC Instance Variable Object. Other Instance Variable Objects are NOT changed.

Can Detect the Scope of a Variable by 
 A useful technique to find out the scope of a variable is to use the defined? method. defined? will return the scope of the variable referenced, or nil if the variable is not defined in the current context.
Global Variable 
defined? $variable_name 
=> "global-variable"

Local Variable
defined? variable_name
=> "local-variable"
defined? _variable_name
=> "local-variable"

Class Variable
defined? @@variable_name 
=> "class-variable"

Instance Variable
defined? @variable_name
=> "instance-variable"

