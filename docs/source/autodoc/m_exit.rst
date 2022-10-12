m_exit
======

string query_default_exit_message()
-----------------------------------

Return the default exit message

string query_default_enter_message()
------------------------------------

Return the default enter message

void set_default_exit_message(mixed arg)
----------------------------------------

The argument can be a string, or function pointer 

void set_default_enter_message(mixed arg)
-----------------------------------------

The argument can be a string or function pointer

void set_default_check(mixed arg)
---------------------------------

those

mixed query_default_check()
---------------------------

Return the defaut check

void set_default_error(mixed value)
-----------------------------------

returning a string.

int has_default_error()
-----------------------

Return true if the room has a default exit error

string query_default_error()
----------------------------

Returns the error default error message.

varargs string *query_exit_directions(int show_hidden)
------------------------------------------------------

list.  If nonnull, they are included

string show_exits()
-------------------

Return a string giving the names of exits for the obvious exits line

string query_enter_msg(string direction)
----------------------------------------

Return the enter messages of a given exit

void set_enter_msg(string direction, mixed *message...)
-------------------------------------------------------

If multiple messages are passed, a random one will be selected when invoked

void add_enter_msg(string direction, mixed *message...)
-------------------------------------------------------

If multiple messages are passed, a random one will be selected when invoked

void remove_enter_msg(string direction, mixed *message...)
----------------------------------------------------------

Remove an enter emssage from a given exit.

mixed *list_enter_msgs(string direction)
----------------------------------------

Return all possible enter messages for a given exit

string query_exit_msg(string direction)
---------------------------------------

Return the exit messages of a given exit

void set_exit_msg(string direction, mixed *message...)
------------------------------------------------------

This message will be displayed in the room the body is leaving

void add_exit_msg(string direction, mixed *message...)
------------------------------------------------------

The message can be a function pointer or a string

void remove_exit_msg(string direction, mixed *message...)
---------------------------------------------------------

Remove an exit emssage from a given exit.

mixed *list_exit_msgs(string direction)
---------------------------------------

List all of the possible exit messages for an exit

varargs string query_exit_destination(string arg)
-------------------------------------------------

Return the destination path of the given exit.

string query_exit_description(string direction)
-----------------------------------------------

Returns the description of the given exit.

void set_exit_description(string direction, mixed description)
--------------------------------------------------------------

Set the description of an exit.

mixed query_exit_check(string direction)
----------------------------------------

Return whether or not the exit can be passed through

void set_exit_check(string direction, function f)
-------------------------------------------------

Function setting the check funciton for the exit

void delete_exit(mixed direction)
---------------------------------

name.

varargs void add_exit(mixed direction, mixed destination)
---------------------------------------------------------

described in the exits doc.

void set_exits( mapping new_exits )
-----------------------------------

exits doc

void set_hidden_exits( string *exits_list ... )
-----------------------------------------------

will be marked as hidden regardless to the rest of the arguments.

void add_hidden_exit( string *exits_list ... )
----------------------------------------------

Make a given exit direction a hidden exit.  See set_hidden_exits

void remove_hidden_exit( string *exits_list ... )
-------------------------------------------------

Make a given exit direction no longer a hidden exit.  See set_hidden_exits

string *query_hidden_exits()
----------------------------

Return all of the hidden exits controlled by the exit object

mapping debug_exits()
---------------------

Return all of the exit info contained within the object

string query_base()
-------------------

Return the evaluated string which is the directory the object is in.

void set_base(mixed what)
-------------------------

Set the base directory to be used by the exits of the environment.