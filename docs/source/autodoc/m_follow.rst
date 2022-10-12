m_follow
========

void set_follow_search(mixed *follow...)
----------------------------------------

is passed to the function pointer.

void add_follow_search(mixed *follow...)
----------------------------------------

See set_follow_search

void remove_follow_search(mixed *follow...)
-------------------------------------------

Remove names from the list of objects that the object will follow

void clear_follow_search()
--------------------------

Clears the search array for following

string *query_follow_search()
-----------------------------

Returns a list of the follow strings that the object will follow

void set_follow(object what)
----------------------------

Set the follow that the object will follow

void clear_follow()
-------------------

Clear the follow that's the object will follow

object query_follow()
---------------------

Returns the follow object which the object is currently following

int is_following()
------------------

Returns 1 if the object is currently following another object

void acquire_follow()
---------------------

perhaps add something to allow weighted choices for follows?

int is_potential_follow(object ob)
----------------------------------

Return 1 if the object is a potential follow to follow