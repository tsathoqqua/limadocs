overrides
=========

string terminal_colour(string str, mapping m, int wrap, int indent)
-------------------------------------------------------------------

Override of efun to avoid some driver issues.

nomask varargs void ed(string file, mixed func)
-----------------------------------------------

functionality.  See ed_session.c

nomask int exec(object target, object src)
------------------------------------------

reconnected.

nomask string debug_info(int operation, object ob)
--------------------------------------------------

to use the valid_bind() apply.

nomask varargs int input_to()
-----------------------------

instead.  See the modal_push() and modal_func() routines.

nomask object find_player(string str)
-------------------------------------

respectively.

nomask object this_player(string str)
-------------------------------------

respectively.

nomask varargs void destruct(object ob, mixed arg)
--------------------------------------------------

argument to indicate that a new copy will be reloaded immediately.

nomask void shutdown()
----------------------

down the mud.

nomask object query_snoop(object ob)
------------------------------------

The query_snoop efun makes no sense in the context of our snoop system.

nomask object query_snooping(object ob)
---------------------------------------

The query_snooping efun makes no sense in the context of our snoop system.

void say(string m)
------------------

instead of say().