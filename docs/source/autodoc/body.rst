body
====

int is_body()
-------------

Is this a body object? 

nomask object query_link()
--------------------------

Return our link object

nomask string query_plan()
--------------------------

Returns our plan

nomask void set_plan(string new_plan)
-------------------------------------

Sets our plan

void save_me()
--------------

Saves us :-)

void remove()
-------------

Handle mailboxes and the last login daemon, as well as the normal stuff

void quit()
-----------

Quit the game.

void net_dead()
---------------

This function is called when we lose our link

void reconnect(object new_link)
-------------------------------

This function is called when we get our link back

int id(string arg)
------------------

id(s) returns 1 if we respond to the name 's'