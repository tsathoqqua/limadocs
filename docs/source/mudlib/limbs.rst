***************
Mudlib *limbs*
***************

Documentation for the limbs mudlib in */std/adversary/health/limbs.c*.

Functions
=========



.. c:function:: int check_body_style()

int check_body_style()
Checks if this body has a body defined already (returns 1), otherwise
returns 0 and adds a humanoid body. This can be replaced with
update_body_style(string body_style) should that be needed.



.. c:function:: int hp_adjustment(int hp, int level)

int hp_adjustment(int hp,int level)
Returns the adjustment HP for a limb for an adversary.



.. c:function:: int update_body_style(string bstyle)

int update_body_style(string body_style);
Queries BODY_D for the number and type of limbs that will be used.
e.g. update_body_style("humanoid") will give the body a torso, head,
two arms, and two legs.
Returns 0 if the body style doesn't exist or if it doesn't contain
at least one vital or system limb.



.. c:function:: string *query_limbs()

string *query_limbs();
Returns a string *containing all limbs that health is applied to.



.. c:function:: string *query_wielding_limbs()

string *query_wielding_limbs();
Returns a string *containing all the limbs that can wield weapons.



.. c:function:: string *query_attacking_limbs()

string *query_attacking_limbs();
Returns a string *containing all the limba that can attack.



.. c:function:: string *query_vital_limbs()

string *query_vital_limbs();
Returns a string *containing all the limbs that are considered
vital for survival. If any one of these limbs is disabled, the
adversary dies.



.. c:function:: string *query_mobile_limbs()

string *query_mobile_limbs();
Lima doesn't do anything with mobile limbs, but they're provided for
those who want health of mobile limbs to affect movement and such.



.. c:function:: string *query_system_limbs()

string *query_system_limbs();
Returns a string *of 'system' limbs. When ALL system limbs are
disabled, the adversary dies.



.. c:function:: string *query_non_limbs()

string *query_non_limbs();
Returns a list of body parts that are not worth tracking health for.
Such body parts are defined by having a max_health of -1.



.. c:function:: void set_max_limb_health(string limb, int x)

void set_max_limb_health(string limb, int x);
Sets the maximum health for a given limb.



.. c:function:: void set_max_health(int x)

void set_max_health(int x);
Set the maximum number of hit points of a monster, and also set it's
hit points to the new max



.. c:function:: void kill_us()

void kill_us();
This functions handles quite a bit:
* Adds XP to the slayer
* calls slain_by(player) in this object.
* Updates the slayers BESTIARY
* Updates opponents Karma
* and finally kills us. (die()).

Yeah, sorry, we had to.



.. c:function:: void disable_limb(string limb)

void disable_limb(string limb);
Disables a limb. For effects on vital and system limbs, see
query_vital_limbs() and query_system_limbs().



.. c:function:: void enable_limb(string limb)

void enable_limb(string limb);
Re-enables a disabled limb.



.. c:function:: varargs int hurt_us(int x, string limb)

varargs int hurt_us(int x, string limb);
Hurt us a specified amount.



.. c:function:: void heal_limb(string limb, int x)

protected void heal_limb(string limb, int x);
Heal us a specified amount, truncating at max_health.



.. c:function:: int is_limb(string s)

int is_limb(string s);
Returns 1 if 's' is a valid limb.



.. c:function:: varargs int query_max_health(string limb)

varargs int query_max_health(string limb);
Tells us the maximum health of a given limb.



.. c:function:: varargs void heal_us(int x, string limb)

varargs void heal_us(int x, string limb);
Heals all limbs by 'x' amount.



.. c:function:: void reincarnate()

void reincarnate();
Makes us alive again!



.. c:function:: int query_health(string limb)

int query_health(string limb);
Find the current number of hitpoints of a monster



.. c:function:: mapping get_health()

mapping get_health();
Return the health mapping for adversary.



.. c:function:: int badly_wounded()

int badly_wounded();
Returns 1 if we're near death.



.. c:function:: int can_move()

int can_move();
Returns 1 if we can move, 0 if not.



.. c:function:: int query_concentration()

int query_concentration()
Returns the amount of concentration currently had by the adversary.



.. c:function:: int max_concentration()

int max_concentration()
Returns the max concentration based on the mana stat and a bonus for level of the
adversary.



.. c:function:: void set_concentration(int mp)

void set_concentration(int mp)
Set the concentration to an integer, but never higher than max_concentration().



.. c:function:: int spend_concentration(int m)

void spend_concentration(int m)
Spends concentration nomatter whether there is enough or too little. concentration is left at 0 no matter
what. Returns 1 if we had enough, 0 if we didn't.



.. c:function:: int use_concentration(int m)

int use_concentration(int m)
Uses concentration from the concentration pool only if it's available and returns 1. If there is not enough
nothing is used, and 0 is returned.



.. c:function:: void restore_concentration(int x)

protected void restore_concentration(int x);
Restore us a specified amount, truncating at max_concentration().



.. c:function:: void heal_all()

void heal_all();
Heal us entirely.


*File generated by LIMA reStructured Text daemon.*
