************************
Module *m_complex_exit*
************************

Documentation for the m_complex_exit module in */std/modules/m_complex_exit.c*.

Functions
=========



.. c:function:: int is_exit()

Return whether or not the object is an exit. 



.. c:function:: int prime_method(string method)

This function merely returns nonzero if the argument is a prime method.  More
specifically this means that the method is one of the root methods that 
others can be built off of.



.. c:function:: void set_base(mixed foo)

Set the base directory from which relative paths are figured
The argument can be a string (directory), object, or function pointer
evaluating to one of these.



.. c:function:: mixed query_base()

Return the evaluated directory string or object from which relative paths 
are figured



.. c:function:: void set_hidden(int i)

Set whether or not the exit is going to show up in the obvious exits list. 
Possible arguments are an int, or a function pointer.



.. c:function:: int query_hidden()

Return 1 if the exit is not to show up as an obvious exit, or 0



.. c:function:: void set_obvious_description(mixed desc)

Set the obvious exit description.  This is the string that will appear in 
the obvious exits line of a room description.  It can be either a function
pointer or a string.



.. c:function:: mixed query_obvious_description()

Return the unevaluated obvious description. 



.. c:function:: string query_direction()

Return the diretion of the exit, that is to show up for obvious exits.



.. c:function:: varargs void set_method(string method,mixed destination,mixed checks,mixed *exit_messages,mixed *enter_messages)

Setup a go method.  The first argument is the verb to be used, the second 
argument is the destination to move the body to when invoked successfully,
the third argument is the check to be performed before invoking the method
the fourth argument is an array of possible exit messages, and the fifth
argument is an array of possible enter messages.  Only the first two
arguments are required, the rest are optional.



.. c:function:: varargs void add_method(string method,mixed destination,mixed checks,mixed *exit_messages,mixed *enter_messages)

Add a go method.  The first argument is the verb to be used, the second 
argument is the destination to move the body to when invoked successfully,
the third argument is the check to be performed before invoking the method
the fourth argument is an array of possible exit messages, and the fifth
argument is an array of possible enter messages.  Only the first two
arguments are required, the rest are optional.



.. c:function:: void remove_method(string method)

Remove a go method from the exit



.. c:function:: string *list_methods()

Return a list of arrays of all of the current go methods of the exit



.. c:function:: int has_method(string method)

Return true if the method exists



.. c:function:: void set_method_checks(string method,mixed checks)

Set the checks for a method.  The checks can be either an int, function
pointer, or string.



.. c:function:: mixed query_method_checks(string method)

Return the evaluated checks on the given method.
If 1 is returned, the checks is successful, 0 - the checks is a failure and 
the parser will generate an error (maybe), else, a string should be returned
which is the error message received by the body.



.. c:function:: void set_method_destination(string method,mixed destination)

Set the destination for a given method
The first argument is the method to have the destination assigned, and
the second argument is either a string or function pointer which will return
a string.



.. c:function:: mixed query_method_destination(string method)

Return the evaluated destination for the given method.
The argument is the method being checked



.. c:function:: varargs void set_method_enter_messages(string method,mixed *messages...)

Set the enter messages to be used by the given method.  
Acceptable arguments are strings, or function pointers, or an array of 
either (mixed is acceptable)
The method is to be seen by the bodies in the room that the body is entering



.. c:function:: varargs void add_method_enter_messages(string method,mixed messages...)

Add the enter messages to be used by the given method.  
Acceptable arguments are strings, or function pointers, or an array of 
either (mixed is acceptable)
The method is to be seen by the bodies in the room that the body is entering



.. c:function:: varargs void remove_method_enter_messages(string method,mixed messages...)

Remove the enter messages to be used by the given method.  
Acceptable arguments are strings, or function pointers, or an array of 
either (mixed is acceptable)
The method is to be seen by the bodies in the room that the body is entering



.. c:function:: string query_method_enter_message(string method)

Return a random method enter message
The method is to be seen by the bodies in the room that the body is entering



.. c:function:: mixed *list_method_enter_messages(string method)

Return an array of the method's enter messages



.. c:function:: varargs void set_method_exit_messages(string method,mixed messages...)

Set the exit messages to be used by the given method.  
Acceptable arguments are strings, or function pointers, or an array of 
either (mixed is acceptable)
The method is to be seen by the bodies in the room that the body is exiting



.. c:function:: varargs void add_method_exit_messages(string method,mixed messages...)

Add the exit messages to be used by the given method.  
Acceptable arguments are strings, or function pointers, or an array of 
either (mixed is acceptable)
The method is to be seen by the bodies in the room that the body is exiting



.. c:function:: varargs void remove_method_exit_messages(string method,mixed messages...)

Remove the exit messages to be used by the given method.  
Acceptable arguments are strings, or function pointers, or an array of 
either (mixed is acceptable)
The method is to be seen by the bodies in the room that the body is exiting



.. c:function:: mixed *list_method_exit_messages(string method)

Return an array of the method's exit messages


*File generated by reStructured Text daemon.*
