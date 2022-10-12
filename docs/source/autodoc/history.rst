history
=======

void set_reply(string o)
------------------------

set_reply(s) sets the person to whom 'reply' goes to.

string query_reply()
--------------------

query the person to whom reply goes tohistory
=======

nomask protected mixed get_nth_item(int n)
------------------------------------------

returns the nth command if it's still in the history buffer.

protected void add_history_item(string item)
--------------------------------------------

add a history item to a history buffer.

protected int get_buffer_size()
-------------------------------

returns the size of a history buffer.

protected int get_command_number()
----------------------------------

returns how many commands have been added to the history.

protected string* get_ordered_history()
---------------------------------------

returns the history buffer in order of least to most recent items

protected int set_history_buffer_size(int s)
--------------------------------------------

sets the size of a history buffer.  -1 means no size limit.

pattern_history_match(string rgx)
---------------------------------

the regexp.  An implicit ^ is added to the beginning of the regexp.