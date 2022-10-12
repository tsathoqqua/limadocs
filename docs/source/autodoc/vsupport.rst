vsupport
========

mixed indirect_get_obj_from_wrd_obj(object ob1, string rel, object ob2)
-----------------------------------------------------------------------

From WRD checks what relation object 1 is in.vsupport
========

mixed check_permission(string what)
-----------------------------------

in other player's/monster's inventories.

int default_object_checks()
---------------------------

make sure that the object is visible to the enactor, amongst other things.

mixed direct_verb_rule(string verb,string rule,mixed args...)
-------------------------------------------------------------

The default method of handling direct objects with verbs.

int do_verb_rule(string verb,string rule,mixed args...)
-------------------------------------------------------

The default handling for all verbs.

mixed direct_get_obj(object ob)
-------------------------------

Do some checks for the parser when we are the OBJ of the "get OBJ" rule

mixed direct_get_obj_from_obj(object ob1, object ob2)
-----------------------------------------------------

From doesn't care what relation Object 1 is in.

mixed direct_get_obj_from_wrd_obj(object ob1, string rel, object ob2)
---------------------------------------------------------------------

Leave the relation checks to indirect_

mixed direct_put_obj_wrd_obj(object ob1, object ob2)
----------------------------------------------------

Handle parser checks for "put OBJ WRD OBJ"     

mixed direct_get_obj_with_obj(object ob1, object ob2)
-----------------------------------------------------

Handle parser checks for "get OBJ with OBJ"

mixed need_to_have()
--------------------

Most of the work is done in try_to_acquire.

mixed direct_look_at_obj(object ob)
-----------------------------------

or it's not visible to the looker, return 0.

mixed direct_look_obj(object ob)
--------------------------------

or it's not visible to the looker, return 0.

mixed direct_look_for_obj(object ob)
------------------------------------

Always allow "look for OBJ" to succeed by default.

mixed direct_look_wrd_obj(object ob)
------------------------------------

Set "look WRD OBJ" to fail by default.

mixed direct_sell_obj(object ob)
--------------------------------

Handle parser checks for "sell OBJ"

mixed direct_smell_obj(object ob)
---------------------------------

Handle parser checks for "smell OBJ" rule.

mixed direct_give_obj_to_liv( object obj, object liv )
------------------------------------------------------

being given.

mixed direct_eat_obj(object ob)
-------------------------------

Handle parser checks for "eat OBJ" rule.

mixed direct_sell_obj_to_liv(object ob, object liv, mixed foo)
--------------------------------------------------------------

Handle parser checks for "sell OBJ to LIV"

mixed direct_buy_obj_from_liv(object ob, object liv)
----------------------------------------------------

Handle parser checks for "buy OBJ from LIV"

mixed direct_drop_obj(object ob)
--------------------------------

Handle parser checks for "drop OBJ" 

mixed direct_flip_obj(object ob)
--------------------------------

Handle parser checks for "flip OBJ"

mixed direct_throw_obj(object ob1, object ob2)
----------------------------------------------

Handle parser checks for "throw OBJ"

mixed direct_throw_obj_at_obj(object ob1, object ob2)
-----------------------------------------------------

Handle parser checks for "throw OBJ at OBJ"

mixed indirect_throw_obj_at_obj(object ob1, object ob2)
-------------------------------------------------------

Handle parser checks for "throw OBJ at OBJ"

mixed direct_pull_obj( object ob )
----------------------------------

 Handle parser checks for "pull OBJ"

mixed direct_press_obj( object ob )
-----------------------------------

 Parser check for "press OBJ"

mixed direct_search_obj( object ob )
------------------------------------

 Parser check for "search OBJ"

mixed direct_search_obj_for_obj( object ob1, object ob2 )
---------------------------------------------------------

Default

mixed indirect_search_obj_for_obj( object ob1, object ob2 )
-----------------------------------------------------------

Default

mixed indirect_search_obj_with_obj( object ob1, object ob2 )
------------------------------------------------------------

Default.

mixed direct_search_obj_with_obj( object ob1, object ob2 )
----------------------------------------------------------

Default

mixed direct_search_for_str_in_obj( string str, object ob )
-----------------------------------------------------------

Default

mixed direct_search_obj_for_str( object ob, string str )
--------------------------------------------------------

Default

mixed direct_search_obj_with_obj_for_str( object ob, string str )
-----------------------------------------------------------------

Default

mixed indirect_search_obj_with_obj_for_str( object ob1, object ob2,  string str )
---------------------------------------------------------------------------------

Default

mixed direct_search_for_str_in_obj_with_obj( string str, object ob1, object ob2 )
---------------------------------------------------------------------------------

Default

mixed indirect_search_for_str_in_obj_with_obj( string str, object ob1, object ob2 )
-----------------------------------------------------------------------------------

Default 

mixed direct_search_obj_for_str_with_obj( object ob1, string str, object ob2 )
------------------------------------------------------------------------------

Default

mixed indirect_search_obj_for_str_with_obj( object ob1, string str, object ob2 )
--------------------------------------------------------------------------------

Default

mixed direct_listen_to_obj( object obj )
----------------------------------------

Default