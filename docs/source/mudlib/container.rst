*******************
Mudlib *container*
*******************

Documentation for the container mudlib in */std/container.c*.

Functions
=========



.. c:function:: string query_relation(object ob)

Return the relation which the given object is conatained by



.. c:function:: varargs void add_relation(string relation, int max_capacity, int hidden)

Add a relation to the complex container.



.. c:function:: void remove_relations(string *rels...)

Remove relations from an object.  Relations can only successfully be removed
if they are unoccupied.



.. c:function:: void set_relations(string *rels...)

Set the relations which are legal for a complex container.  Example:
set_relations("on", "in", "under").  The first one is the default
relation (the one used by set_objects(), etc)



.. c:function:: string *get_relations()

Return all of the possible relations for an object



.. c:function:: string is_relation_alias(string test)

Determine whether or not the relation is valid and return which relation
the alias is associated with.



.. c:function:: void set_relation_alias(string relation, string *aliases...)

Set the aliases that a relation has



.. c:function:: void add_relation_alias(string relation, string *aliases...)

Add additional aliases that a relation has.



.. c:function:: string *query_relation_aliases(string relation)

Return the array of aliases that a relation has.



.. c:function:: mapping list_relation_aliases()

List all of the relation alias information



.. c:function:: void set_default_relation(string set)

Sets the default relation for the container.  This relation is used if no
relation is specified on many functions



.. c:function:: string query_default_relation()

Returns the default relation for the container.  See set_default_relation.



.. c:function:: varargs float query_capacity(string relation)

Returns the amount of mass currently attached to a container



.. c:function:: varargs void set_max_capacity(int cap, string relation)

Set the maximum capacity for a given relation.



.. c:function:: varargs int query_max_capacity(string relation)

Returns the maximum capacity for a given relation



.. c:function:: int query_total_capacity()

Returns the capacity directly attributed to the container.  This should
normally include anything attached or within the container.



.. c:function:: int query_mass()




.. c:function:: mixed receive_object(object target, string relation)

Determine whether we will accept having an object moved into us;
returns a value from <move.h> if there is an error



.. c:function:: varargs mixed release_object(object target, int force)

Prepare for an object to be moved out of us; the object isn't allowed
to leave if we return zero or a string (error message)



.. c:function:: string look_in(string relation)

returns a string containing the result of looking inside (or optionally
a different relation) of the object



.. c:function:: string simple_long()

Return the long description without the inventory list.



.. c:function:: mixed ob_state()

Determine whether an object should be grouped with other objects of the
same kind as it.  -1 is unique, otherwise if objects will be grouped
according to the return value of the function.



.. c:function:: int parent_environment_accessible()

Return 1 if the parser should include the outside world in its
decisions, overloaded in non_room descendants



.. c:function:: int inventory_visible()

Return 1 if the contents of this object can be seen, zero otherwise



.. c:function:: varargs mixed *set_objects(mapping m, string relation)

Provide a list of objects to be loaded now and at every reset.  The key
should be the filename of the object, and the value should be the number
of objects to clone.  The value can also be an array, in which case the
first element is the number of objects to clone, and the remaining elements
are arguments that should be passed to create() when the objects are cloned.
An optional second string argument represents a specific relation which
should produce objects on reset()

Note:  the number already present is determined by counting the number of
objects with the same first id, and objects are only cloned to bring the
count up to that number.

set_objects((["torch"]:5)); - five torches
set_objects((["door"]:({"west","room2"}))); - Door with 2 arguments
                                              passed to setup.
set_objects((["door"]:({2,({"west","room2"}),
                      ({"east","room3"})
                      })));




.. c:function:: varargs mixed *set_unique_objects(mapping m, string relation)

Provide a list of objects to be loaded now and at every reset if they
are not already loaded.  The key should be the filename of the object,
and the value should be an array which is passed to create() when the
objects are cloned.
The structure of the mapping should be the same as the structure of the
mapping for set_objects().  For unique objects, to be checked, you should
have a function in the object called test_unique() which will return 1 if
uniqueness requirements are met.  The prototype for the function is
        int test_unique();
An optional second string argument represents a specific relation which
should produce objects on reset()



.. c:function:: varargs string introduce_contents(string relation)

returns a string appropriate for introduction the contents of an object
in room descriptions.



.. c:function:: int inventory_accessible()

Return 1 if the contents of this object can be touched, manipulated, etc



.. c:function:: int is_container()

Returns 1 if an object is a container


*File generated by LIMA reStructured Text daemon.*
