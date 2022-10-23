******************
Mudlib *vsupport*
******************

Documentation for the vsupport mudlib in */std/object/vsupport.c*.

Functions
=========



.. c:function:: mixed check_permission(string what)

check_permission(what) calls who->allow(what) in our owner, if one exists.
Used by many of the verb routines to keep players from fiddling with things
in other player's/monster's inventories.



.. c:function:: int default_object_checks()

Nearly all direct/indirect parsing functions should call this.  It checks to 
make sure that the object is visible to the enactor, amongst other things.



.. c:function:: mixed direct_verb_rule(string verb,string rule,mixed args...)

The default method of handling direct objects with verbs.



.. c:function:: int do_verb_rule(string verb,string rule,mixed args...)

The default handling for all verbs.



.. c:function:: mixed direct_get_obj(object ob)

Do some checks for the parser when we are the OBJ of the "get OBJ" rule



.. c:function:: mixed direct_get_obj_from_obj(object ob1, object ob2)

Handle parser checks for "get OBJ from OBJ"
From doesn't care what relation Object 1 is in.



.. c:function:: mixed direct_get_obj_from_wrd_obj(object ob1, string rel, object ob2)

Handle parser checks for "get OBJ from WRD OBJ"
Leave the relation checks to indirect_



.. c:function:: mixed direct_put_obj_wrd_obj(object ob1, object ob2)

Handle parser checks for "put OBJ WRD OBJ"     



.. c:function:: mixed direct_get_obj_with_obj(object ob1, object ob2)

Handle parser checks for "get OBJ with OBJ"



.. c:function:: mixed need_to_have()

Do some sanity checks for verbs that auto-take objects, or only allow
you to use objects you are carrying.
Most of the work is done in try_to_acquire.



.. c:function:: mixed direct_look_at_obj(object ob)

Whether or not an object can be looked at.  If there's no short description, 
or it's not visible to the looker, return 0.



.. c:function:: mixed direct_look_obj(object ob)

Whether or not an object can be looked at.  If there's no short description, 
or it's not visible to the looker, return 0.



.. c:function:: mixed direct_look_for_obj(object ob)

Always allow "look for OBJ" to succeed by default.



.. c:function:: mixed direct_look_wrd_obj(object ob)

Set "look WRD OBJ" to fail by default.



.. c:function:: mixed direct_sell_obj(object ob)

Handle parser checks for "sell OBJ"



.. c:function:: mixed direct_smell_obj(object ob)

Handle parser checks for "smell OBJ" rule.



.. c:function:: mixed direct_give_obj_to_liv( object obj, object liv )

Handle parser checks for "give OBJ to LIV" rule, where we are the object
being given.



.. c:function:: mixed direct_eat_obj(object ob)

Handle parser checks for "eat OBJ" rule.



.. c:function:: mixed direct_sell_obj_to_liv(object ob, object liv, mixed foo)

Handle parser checks for "sell OBJ to LIV"



.. c:function:: mixed direct_buy_obj_from_liv(object ob, object liv)

Handle parser checks for "buy OBJ from LIV"



.. c:function:: mixed direct_drop_obj(object ob)

Handle parser checks for "drop OBJ" 



.. c:function:: mixed direct_flip_obj(object ob)

Handle parser checks for "flip OBJ"



.. c:function:: mixed direct_throw_obj(object ob1, object ob2)

Handle parser checks for "throw OBJ"



.. c:function:: mixed direct_throw_obj_at_obj(object ob1, object ob2)

Handle parser checks for "throw OBJ at OBJ"



.. c:function:: mixed indirect_throw_obj_at_obj(object ob1, object ob2)

Handle parser checks for "throw OBJ at OBJ"



.. c:function:: mixed direct_pull_obj( object ob )

Handle parser checks for "pull OBJ"



.. c:function:: mixed direct_press_obj( object ob )

Parser check for "press OBJ"



.. c:function:: mixed direct_search_obj( object ob )

Parser check for "search OBJ"



.. c:function:: mixed direct_search_obj_for_obj( object ob1, object ob2 )

Default



.. c:function:: mixed indirect_search_obj_for_obj( object ob1, object ob2 )

Default



.. c:function:: mixed indirect_search_obj_with_obj( object ob1, object ob2 )

Default.



.. c:function:: mixed direct_search_obj_with_obj( object ob1, object ob2 )

Default



.. c:function:: mixed direct_search_for_str_in_obj( string str, object ob )

Default



.. c:function:: mixed direct_search_obj_for_str( object ob, string str )

Default



.. c:function:: mixed direct_search_obj_with_obj_for_str( object ob, string str )

Default



.. c:function:: mixed indirect_search_obj_with_obj_for_str( object ob1, object ob2,  string str )




.. c:function:: mixed direct_search_for_str_in_obj_with_obj( string str, object ob1, object ob2 )

Default



.. c:function:: mixed indirect_search_for_str_in_obj_with_obj( string str, object ob1, object ob2 )

Default 



.. c:function:: mixed direct_search_obj_for_str_with_obj( object ob1, string str, object ob2 )

Default



.. c:function:: mixed indirect_search_obj_for_str_with_obj( object ob1, string str, object ob2 )

Default



.. c:function:: mixed direct_listen_to_obj( object obj )

Default

.. note:: shouldn't these to only be in coins? (line 213)
.. note:: This DEFINATELY shouldn't be here.  Should be in living.c (line 219)

*File generated by LIMA reStructured Text daemon.*
