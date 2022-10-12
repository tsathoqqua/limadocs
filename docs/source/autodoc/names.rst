names
=====

nomask void set_proper_name(string str)
---------------------------------------

adjectives added in front of their names.

void set_unique(int x)
----------------------

Unique objects are always refered to as 'the ...' and never 'a ...'

int query_unique()
------------------

Return the value of 'unique'

void set_plural( int x )
------------------------

Plural objects are referred to as "the", not "a"

int query_plural()
------------------

Return the value of plural

string short()
--------------

it should be refered to

string the_short()
------------------

return the short descriptions, with the word 'the' in front if appropriate

string a_short()
----------------

return the short descriptions, with the word 'a' in front if appropriate

add_adj(string *adj... )
------------------------

:primary adjective when using add_adj().

void add_plural( string *plural... )
------------------------------------

Add a plural id

void add_id_no_plural( string *id... )
--------------------------------------

Add an id without adding the corresponding plural

void add_id( string *id... )
----------------------------

Add an id and its corresponding plural

void remove_id( string *id... )
-------------------------------

Remove the given id

void clear_id()
---------------

removes all the ids of an object.

void clear_adj()
----------------

Remove all the adjectives from an object

string *query_id()
------------------

Returns an array containing the ids of an object

string query_primary_id()
-------------------------

Returns the primary id of an object

string query_primary_adj()
--------------------------

Returns the primary adj of an object

string query_primary_name()
---------------------------

Returns the primary name (primary adj + primary id) of an object

string *query_adj()
-------------------

return the adjectives