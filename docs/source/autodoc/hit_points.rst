hit_points
==========

void set_max_health(int x)
--------------------------

 hit points to the new max.

void kill_us()
--------------

 Call this function to kill an adversary completely.

void set_health(int x)
----------------------

 see also: set_max_health

varargs int hurt_us(int x, string unused)
-----------------------------------------

 Hurt us a specified amount.

void heal_us(int x)
-------------------

 Heal us a specified amount, truncating at max_health.

void reincarnate()
------------------

 Makes us alive again!

void update_health()
--------------------

 Correct the health if necessary 

varargs int query_health(string unused)
---------------------------------------

 Find the current number of hitpoints of a monster

int query_max_health()
----------------------

 Find the maximum number of hitpoints of a monster.

void heal_all()
---------------

 Heal us completely.

int badly_wounded()
-------------------

 Returns 1 if we're nearing death.