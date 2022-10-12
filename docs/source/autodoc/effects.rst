effects
=======

int time_to_next_effect()
-------------------------

 Returns time remaining in call_out to the next effect

int actual_period( mixed val )
------------------------------

           function : evaluate it

void remove_effect_at(int pos)
------------------------------

 Removes effect from queue at appropriate point

int find_effect_index(string ob)
--------------------------------

 Return -1 on failure

int find_effect_name_index(string name)
---------------------------------------

 Return -1 on failure

int *find_effect_indexes_matching(string name)
----------------------------------------------

 Return ({})

int find_effect(string ob)
--------------------------

 Return 0 on failure

mixed query_effect_args(string ob)
----------------------------------

 Return args of specified effect

int remove_effect(string ob)
----------------------------

 Return 1 on success, 0 on failure

int remove_effect_named(string name)
------------------------------------

 Return 1 on success, 0 on failure

int remove_effects_matching(string name)
----------------------------------------

 Return 1 on success, 0 on failure

void insert_effect_at(class effect_class effect, int pos)
---------------------------------------------------------

 Inserts effect into queue at appropriate point

int insert_effect(class effect_class effect)
--------------------------------------------

 Returns 1 on success, 0 on failure.

void next_effect()
------------------

 Then call out to next effect in the queue

void clear_effects()
--------------------

 Clears the effects queue

mixed *query_effects()
----------------------

 Returns copy of the effects queue

void add_effect(string ob, mixed args, int repeats, mixed interval)
-------------------------------------------------------------------

 interval will default to ob->query_interval()

void reinstate_effects()
------------------------

 Called on relogging to restart effects.