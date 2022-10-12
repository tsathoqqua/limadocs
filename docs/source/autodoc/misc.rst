misc
====

call_trace()
------------

returns the stack of objects and functions

clean_array(mixed* r)
---------------------

entries removed.  Eg, clean_array(({1,2,2}))  => ({1,2})

string *rev_explode(string arr_in, string delim)
------------------------------------------------

but the mudlib already requires that anyway.

int cmp( mixed a, mixed b )
---------------------------

mappings and arrays.

mixed insert( mixed 	to_insert,
	mixed	into_array,
	int	where )
---------------------------------------------------------------

where n is the 3rd argument passed to insert.

varargs mixed eval( string arg, string includefile )
----------------------------------------------------

({1,2,3,4}).

mixed* decompose( mixed* org )
------------------------------

See flatten_array for a recursive version.

mixed choice( mixed f )
-----------------------

 is an aggregate type (i.e., A string, array or mapping).

mixed min( mixed f )
--------------------

or mapping).

mixed max( mixed f )
--------------------

array or mapping.

flatten_array(mixed arr)
------------------------

arrays so that the result is a one dimensional array

void call_out_chain(mixed *funcs, int delay, mixed *args...)
------------------------------------------------------------

another, with each returning the delay till the next one is called.

mixed *sort_by_value(mixed arr, function value_func)
----------------------------------------------------

value of the function fmisc
====

protected void set_didlog_time(int t)
-------------------------------------

Set the last time that the didlog was read for the user.

int query_didlog_time()
-----------------------

Return the time that the user last read the didlog.

void deliver_didlog_message(string str)
---------------------------------------

Deliver a didlog message while someone is still logged in

void set_didlog_off(int x)
--------------------------

If nonzero, this turns off the automatic posting of the didlog for the user

int query_didlog_off()
----------------------

Returns 1 if the user does not receive the automatic posting of the didlog

void display_didlog()
---------------------

Primarily used in the login process.