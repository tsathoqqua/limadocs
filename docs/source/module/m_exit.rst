****************
Module *m_exit*
****************

Documentation for the m_exit module in */std/modules/m_exit.c*.

Functions
=========



.. c:function:: string query_default_exit_message()

Return the default exit message



.. c:function:: string query_default_enter_message()

Return the default enter message



.. c:function:: void set_default_exit_message(mixed arg)

Set the default exit message for all exits.
The argument can be a string, or function pointer



.. c:function:: void set_default_enter_message(mixed arg)

Set the default enter message for all exits
The argument can be a string or function pointer



.. c:function:: void set_default_check(mixed arg)

Set the default check for all exits
The argument can be a 0, 1, string, or function pointer returning one of
those



.. c:function:: mixed query_default_check()

Return the defaut check



.. c:function:: void set_default_error(mixed value)

Set the default error message (the message given when someone goes a
direction with no exit).  This should be a string or a function ptr
returning a string.



.. c:function:: int has_default_error()

Return true if the room has a default exit error



.. c:function:: string query_default_error()

Returns the error default error message.



.. c:function:: varargs string *query_exit_directions(int show_hidden)

Return all of the exit directions controlled by the exit object
The optional argument determines whether hidden exits are included in this
list.  If nonnull, they are included



.. c:function:: string show_exits()

Return a string giving the names of exits for the obvious exits line



.. c:function:: string query_enter_msg(string direction)

Return the enter messages of a given exit



.. c:function:: void set_enter_msg(string direction, mixed *message...)

Set the enter message of a given exit.
This message will be displayed in the destination room.
The message can be a fucntion pointer or a string.
If multiple messages are passed, a random one will be selected when invoked



.. c:function:: void add_enter_msg(string direction, mixed *message...)

Add an additional enter message to a given exit.
The message can be a function pointer or a string
If multiple messages are passed, a random one will be selected when invoked



.. c:function:: void remove_enter_msg(string direction, mixed *message...)

Remove an enter emssage from a given exit.



.. c:function:: mixed *list_enter_msgs(string direction)

Return all possible enter messages for a given exit



.. c:function:: string query_exit_msg(string direction)

Return the exit messages of a given exit



.. c:function:: void set_exit_msg(string direction, mixed *message...)

Set the exit message of a given exit.
This message will be displayed in the room the body is leaving



.. c:function:: void add_exit_msg(string direction, mixed *message...)

Add an additional exit message to a given exit.
The message can be a function pointer or a string



.. c:function:: void remove_exit_msg(string direction, mixed *message...)

Remove an exit emssage from a given exit.



.. c:function:: mixed *list_exit_msgs(string direction)

List all of the possible exit messages for an exit



.. c:function:: varargs string query_exit_destination(string arg)

Return the destination path of the given exit.



.. c:function:: string query_exit_description(string direction)

Returns the description of the given exit.



.. c:function:: void set_exit_description(string direction, mixed description)

Set the description of an exit.



.. c:function:: mixed query_exit_check(string direction)

Return whether or not the exit can be passed through



.. c:function:: void set_exit_check(string direction, function f)

Function setting the check funciton for the exit



.. c:function:: void delete_exit(mixed direction)

Remove a single exit from the room.  The direction should be an exit
name.



.. c:function:: varargs void add_exit(mixed direction, mixed destination)

Add an exit to the object with a destination.  .
Add the value should be a filename or a more complex structure as
described in the exits doc.



.. c:function:: void set_exits(mapping new_exits)

Sets the exit mapping of a room.  The keys should be exit names, the values
should be either filenames or more complex structures described in the
exits doc



.. c:function:: void set_hidden_exits(string *exits_list...)

This is the list of exits to NOT be shown to the mortals in the room.
If "all" is any of the arguements in exits_list all exits for the object
will be marked as hidden regardless to the rest of the arguments.



.. c:function:: void add_hidden_exit(string *exits_list...)

Make a given exit direction a hidden exit.  See set_hidden_exits



.. c:function:: void remove_hidden_exit(string *exits_list...)

Make a given exit direction no longer a hidden exit.  See set_hidden_exits



.. c:function:: string *query_hidden_exits()

Return all of the hidden exits controlled by the exit object



.. c:function:: mapping debug_exits()

Return all of the exit info contained within the object



.. c:function:: string query_base()

Return the evaluated string which is the directory the object is in.



.. c:function:: void set_base(mixed what)

Set the base directory to be used by the exits of the environment.


*File generated by LIMA reStructured Text daemon.*
