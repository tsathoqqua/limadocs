limbs
=====

object *query_armors(string s)
------------------------------

 Returns the armors that are covering limb 's'.

nomask int wear_item(object what, string where)
-----------------------------------------------

 Forces the adversary to wear 'what' on its 'where' limb.

nomask int remove_item(object what, string where)
-------------------------------------------------

 Removes armor 'what' from the 'where' limb.

int has_body_slot(string slot)
------------------------------

 Returns 1 if the body slot is a valid one.

string *query_armor_slots()
---------------------------

 Returns all valid armor slots on an adversary.limbs
=====

int check_body_style()
----------------------

 update_body_style(string body_style) should that be needed.

int hp_adjustment(int hp, int level)
------------------------------------

 Returns the adjustment HP for a limb for an adversary.

int update_body_style(string bstyle)
------------------------------------

 at least one vital or system limb.

string *query_limbs()
---------------------

 Returns a string *containing all limbs that health is applied to.

string *query_wielding_limbs()
------------------------------

 Returns a string *containing all the limbs that can wield weapons.

string *query_attacking_limbs()
-------------------------------

 Returns a string *containing all the limba that can attack.

string *query_vital_limbs()
---------------------------

 adversary dies.

string *query_mobile_limbs()
----------------------------

 those who want health of mobile limbs to affect movement and such.

string *query_system_limbs()
----------------------------

 disabled, the adversary dies.

string *query_non_limbs()
-------------------------

 Such body parts are defined by having a max_health of -1.

void set_max_limb_health(string limb, int x)
--------------------------------------------

 Sets the maximum health for a given limb.

void set_max_health(int x)
--------------------------

 hit points to the new max

void kill_us()
--------------

 Yeah, sorry, we had to.

void disable_limb(string limb)
------------------------------

 query_vital_limbs() and query_system_limbs().

void enable_limb(string limb)
-----------------------------

 Re-enables a disabled limb.

varargs int hurt_us(int x, string limb)
---------------------------------------

 Hurt us a specified amount.

void heal_limb(string limb, int x)
----------------------------------

 Heal us a specified amount, truncating at max_health.

int is_limb(string s)
---------------------

 Returns 1 if 's' is a valid limb.

varargs int query_max_health(string limb)
-----------------------------------------

 Tells us the maximum health of a given limb.

varargs void heal_us(int x, string limb)
----------------------------------------

 Heals all limbs by 'x' amount.

void reincarnate()
------------------

 Makes us alive again!

int query_health(string limb)
-----------------------------

 Find the current number of hitpoints of a monster

mapping get_health()
--------------------

 Return the health mapping for adversary.

int badly_wounded()
-------------------

 Returns 1 if we're near death.

int can_move()
--------------

 Returns 1 if we can move, 0 if not.

int query_concentration()
-------------------------

 Returns the amount of concentration currently had by the adversary.

int max_concentration()
-----------------------

 adversary.

void set_concentration(int mp)
------------------------------

 Set the concentration to an integer, but never higher than max_concentration().

int spend_concentration(int m)
------------------------------

 what. Returns 1 if we had enough, 0 if we didn't.

int use_concentration(int m)
----------------------------

 nothing is used, and 0 is returned.

void restore_concentration(int x)
---------------------------------

 Restore us a specified amount, truncating at max_concentration().

void heal_all()
---------------

 Heal us entirely.