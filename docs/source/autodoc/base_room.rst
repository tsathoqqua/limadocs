base_room
=========

string stat_me()
----------------

as well as the short and exits.

void set_brief(string str)
--------------------------

Set the name of the room seen at the top of the description and in brief mode

int can_hold_water()
--------------------

Return 1 if the object can hold water.

void set_area(string *names...)
-------------------------------

Can either be a string, or an array of strings

string *query_area()
--------------------

Find out what 'areas' the room belongs to.  See set_area.

string long_without_object(object o)
------------------------------------

same long as the room, but not see itself in the description.