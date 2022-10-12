m_complex_exit
==============

int is_exit()
-------------

Return whether or not the object is an exit. 

int prime_method(string method)
-------------------------------

others can be built off of.

void set_base(mixed foo)
------------------------

evaluating to one of these.

mixed query_base()
------------------

are figured

void set_hidden(int i)
----------------------

Possible arguments are an int, or a function pointer.

int query_hidden()
------------------

Return 1 if the exit is not to show up as an obvious exit, or 0

void set_obvious_description(mixed desc)
----------------------------------------

pointer or a string.

mixed query_obvious_description()
---------------------------------

Return the unevaluated obvious description. 

string query_direction()
------------------------

Return the diretion of the exit, that is to show up for obvious exits.

varargs void set_method(string method,mixed destination,mixed checks,mixed *exit_messages,mixed *enter_messages)
---------------------------------------------------------------------------------------

arguments are required, the rest are optional.

varargs void add_method(string method,mixed destination,mixed checks,mixed *exit_messages,mixed *enter_messages)
---------------------------------------------------------------------------------------

arguments are required, the rest are optional.

void remove_method(string method)
---------------------------------

Remove a go method from the exit

string *list_methods()
----------------------

Return a list of arrays of all of the current go methods of the exit

int has_method(string method)
-----------------------------

Return true if the method exists

void set_method_checks(string method,mixed checks)
--------------------------------------------------

pointer, or string.

mixed query_method_checks(string method)
----------------------------------------

which is the error message received by the body.

void set_method_destination(string method,mixed destination)
------------------------------------------------------------

a string.

mixed query_method_destination(string method)
---------------------------------------------

The argument is the method being checked

varargs void set_method_enter_messages(string method,mixed *messages...)
------------------------------------------------------------------------

The method is to be seen by the bodies in the room that the body is entering

varargs void add_method_enter_messages(string method,mixed messages...)
-----------------------------------------------------------------------

The method is to be seen by the bodies in the room that the body is entering

varargs void remove_method_enter_messages(string method,mixed messages...)
--------------------------------------------------------------------------

The method is to be seen by the bodies in the room that the body is entering

string query_method_enter_message(string method)
------------------------------------------------

The method is to be seen by the bodies in the room that the body is entering

mixed *list_method_enter_messages(string method)
------------------------------------------------

Return an array of the method's enter messages

varargs void set_method_exit_messages(string method,mixed messages...)
----------------------------------------------------------------------

The method is to be seen by the bodies in the room that the body is exiting

varargs void add_method_exit_messages(string method,mixed messages...)
----------------------------------------------------------------------

The method is to be seen by the bodies in the room that the body is exiting

varargs void remove_method_exit_messages(string method,mixed messages...)
-------------------------------------------------------------------------

The method is to be seen by the bodies in the room that the body is exiting

mixed *list_method_exit_messages(string method)
-----------------------------------------------

Return an array of the method's exit messages