m_wander
========

void set_wander_area(string *area...)
-------------------------------------

restrictions.

void add_wander_area(string *area...)
-------------------------------------

Add area(s) which an NPC can wander in.  See set_wander_area()

void remove_wander_area(string *area...)
----------------------------------------

Remove area(s) which an NPC can wander in.  See set_wander_area()

void clear_wander_area()
------------------------

this allows the NPC to wander anywhere.  See set_wander_area()

string *query_wander_area()
---------------------------

See set_wander_area()

void set_wander_time(mixed time)
--------------------------------

during runtime.

int query_wander_time()
-----------------------

ZERO_RETURN (refer to the M_WANDER file) will be returned

void set_max_moves(int i)
-------------------------

interaction

int query_max_moves()
---------------------

interaction before stopping.

void cease_wandering()
----------------------

start_wandering() is required to make the NPC move again

void stop_wandering()
---------------------

room.

void start_wandering()
----------------------

Starts an NPC wandering