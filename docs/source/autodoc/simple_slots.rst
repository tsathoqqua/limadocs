simple_slots
============

nomask int wear_item(object what, string sname)
-----------------------------------------------

 Wear item 'what' on armor slot 'sname'.

nomask int remove_item(object what, string sname)
-------------------------------------------------

 Remove armor 'what' from armor slot 'sname'.

nomask int has_body_slot(string what)
-------------------------------------

 Returns 1 if 'what' is a valid armor slot.

string *query_armor_slots()
---------------------------

 Returns an array of all valid armor slots.