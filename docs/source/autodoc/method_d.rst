method_d
========

void add_method(string method,string *equivs...)
------------------------------------------------

Any arguments after the first are equivalents to the method.

void remove_method(string method)
---------------------------------

Remove a method and it's equivalents

string *list_methods()
----------------------

Return an array of all methods which have equivalents

void remove_method_equivalents(string method, string *equivs...)
----------------------------------------------------------------

Remove equivalents from a given method

string *list_method_equivalents(string method)
----------------------------------------------

Return an array of equivalents to a given method