container
=========

string query_relation(object ob)
--------------------------------

Return the relation which the given object is conatained by

varargs void add_relation(string relation,int max_capacity,int hidden)
----------------------------------------------------------------------

Add a relation to the complex container.

void remove_relations(string *rels...)
--------------------------------------

if they are unoccupied.

void set_relations(string *rels...)
-----------------------------------

relation (the one used by set_objects(), etc)

string *get_relations()
-----------------------

Return all of the possible relations for an object

string is_relation_alias(string test)
-------------------------------------

the alias is associated with.

void set_relation_alias(string relation,string aliases...)
----------------------------------------------------------

Set the aliases that a relation has

void add_relation_alias(string relation,string aliases...)
----------------------------------------------------------

Add additional aliases that a relation has.

string *query_relation_aliases(string relation)
-----------------------------------------------

Return the array of aliases that a relation has.

mapping list_relation_aliases()
-------------------------------

List all of the relation alias information

void set_default_relation(string set)
-------------------------------------

relation is specified on many functions

string query_default_relation()
-------------------------------

Returns the default relation for the container.  See set_default_relation.

varargs int query_capacity(string relation)
-------------------------------------------

Returns the amount of mass currently attached to a container

varargs void set_max_capacity(int cap, string relation)
-------------------------------------------------------

Set the maximum capacity for a given relation.

varargs int query_max_capacity(string relation)
-----------------------------------------------

Returns the maximum capacity for a given relation

int query_total_capacity()
--------------------------

normally include anything attached or within the container.

int query_mass()
----------------

normally include anything attached or within the container.

mixed receive_object( object target, string relation )
------------------------------------------------------

returns a value from <move.h> if there is an error

varargs mixed release_object( object target, int force )
--------------------------------------------------------

to leave if we return zero or a string (error message)

string look_in( string relation )
---------------------------------

a different relation) of the object

string simple_long()
--------------------

Return the long description without the inventory list.

mixed ob_state()
----------------

according to the return value of the function.

int parent_environment_accessible()
-----------------------------------

decisions, overloaded in non_room descendants

int inventory_visible()
-----------------------

Return 1 if the contents of this object can be seen, zero otherwise

varargs mixed *set_objects(mapping m,string relation)
-----------------------------------------------------

count up to that number.

varargs mixed *set_unique_objects(mapping m,string relation)
------------------------------------------------------------

should produce objects on reset()

varargs string introduce_contents(string relation)
--------------------------------------------------

in room descriptions.

int inventory_accessible()
--------------------------

Return 1 if the contents of this object can be touched, manipulated, etc

int is_container()
------------------

Returns 1 if an object is a container