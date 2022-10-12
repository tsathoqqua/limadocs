m_fluid_container
=================

varargs int can_hold_fluid( mixed ob )
--------------------------------------

in the types of fluids it can hold.

void set_leak_action( mixed x )
-------------------------------

that is being put into it.

void set_full_action( mixed x )
-------------------------------

my_action().

void move_fluid( object ob )
----------------------------

it fills the container

void part_move_fluid( object ob )
---------------------------------

transferred is limited to the original quantity

int fill_with( object fluid )
-----------------------------

(eg fill from source, such as tap)

int part_fill_with( object fluid )
----------------------------------

(eg fill one container from another, by "pour")