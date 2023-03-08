***************
Mudlib *limbs*
***************

Documentation for the limbs mudlib in */std/adversary/health/limbs.c*.

Functions
=========



.. c:function:: int query_drunk()

Returns the points of drunkeness currently held by this adversary.
See also set_drunk(int).



.. c:function:: int query_max_drunk()

The maximum level of drunk points this adversary can hold. This increases
with level and constitution stat score.



.. c:function:: int query_abuse()

Returns the abuse points held by this adversary.



.. c:function:: int query_max_abuse()

Returns the maximum abuse points held by this adversary before they die
permanently.



.. c:function:: int query_abuse_percent()

Returns the abuse percentage.



.. c:function:: int abuse_body(int a)

Abuse this adversary for a number of points. These are hard to get
rid of so use sparringly.



.. c:function:: void set_drunk(int d)

Set a drunk level at a specific point. This is useful for adversaries
you want to exhibit drunk behavior.



.. c:function:: int drink_alchohol(int d)

Drink alchohol and add 'd' to the drunk level.



.. c:function:: int query_drunk_percent()

Returns the drunk level in percent.



.. c:function:: string drunk_diagnose()

Returns a string with a description of the drunkeness level.



.. c:function:: void sober_up(int s)

Removes a number of drunk points.



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



.. c:function:: int query_reflex()

int query_reflex()
Returns the amount of reflex currently had by the adversary.



.. c:function:: int max_reflex()

int max_reflex()
Returns the max reflex based on the mana stat and a bonus for level of the
adversary.



.. c:function:: void set_reflex(int mp)

void set_reflex(int mp)
Set the reflex to an integer, but never higher than max_reflex().



.. c:function:: int spend_reflex(int m)

void spend_reflex(int m)
Spends reflex nomatter whether there is enough or too little. reflex is left at 0 no matter
what. Returns 1 if we had enough, 0 if we didn't.



.. c:function:: int use_reflex(int m)

int use_reflex(int m)
Uses reflex from the reflex pool only if it's available and returns 1. If there is not enough
nothing is used, and 0 is returned.



.. c:function:: void restore_reflex(int x)

protected void restore_reflex(int x);
Restore us a specified amount, truncating at max_reflex().



.. c:function:: void set_max_limb_health(string limb, int x)

void set_max_limb_health(string limb, int x);
Sets the maximum health for a given limb.



.. c:function:: void set_max_health(int x)

void set_max_health(int x);
Set the maximum number of hit points of a monster, and also set it's
hit points to the new max. MUST be called *after* update_body_type()
if that is called since that resets all limbs to neutral hitpoints
i.e. sums to 100.



.. c:function:: int can_move()

int can_move();
Returns 1 if we can move, 0 if not.



.. c:function:: void kill_us()

void kill_us();
Kills us. =)



.. c:function:: string query_random_limb()

Return a limb based on the size of the limb. The larger
the limb the higher chance it's returned. Only limbs that
have hitpoints are returned.



.. c:function:: void disable_limb(string limb)

void disable_limb(string limb);
Disables a limb. For effects on vital and system limbs, see
query_vital_limbs() and query_system_limbs().



.. c:function:: void enable_limb(string limb)

void enable_limb(string limb);
Re-enables a disabled limb.



.. c:function:: varargs void set_health(string limb, int x)

Set hitpoints for a limb to a certain amount.



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



.. c:function:: void heal_all()

void heal_all();
Heal us entirely.



.. c:function:: void reincarnate()

void reincarnate();
Makes us alive again!



.. c:function:: int query_health(string limb)

int query_health(string limb);
Find the current number of hitpoints of a monster



.. c:function:: mapping get_health()

mapping get_health();
Return the health mapping for adversary.



.. c:function:: varargs mixed *query_worst_limb(int vital)

Returns an array of a limb and a percentage of health that is
the worst hurt vital limb if vital=1, otherwise from all limbs.



.. c:function:: string badly_wounded()

Returns 1 if we're near death.



.. c:function:: string very_wounded()

Returns 1 if we're very wounded (50% hp on vital limbs). Mobs will start drinking and
eating when they hit this level of damage.



.. c:function:: string diagnose()

Returns a string diagnosing the adversary. The string lists limbs in bad health,
 drunkenness level and other conditions that affect the adversary.


*File generated by LIMA reStructured Text daemon.*
