door
====

void update_state(object ob)
----------------------------

 Update the state of both this door and its sibling.

void update_sibling()
---------------------

 See also m_sibling::update_sibling()

void do_on_close()
------------------

 Called when the door is closed.

void set_door_direction(string direction)
-----------------------------------------

 resulting exit.

void set_door_destination(mixed dest)
-------------------------------------

 anything M_COMPLEX_EXIT::set_method()'s destination argument will accept

mixed query_door_destination()
------------------------------

 Return the destination of the door

void setup_exits(string dir,string room)
----------------------------------------

                instead.

void setup_door(string ident, string dir, string room)
------------------------------------------------------

                set_sibling_ident() instead.