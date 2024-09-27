################
Creating weapons
################

Weapons in LIMA all inherit ``M_DAMAGE_SOURCE``, meaning something that can be a source
of damage. So even things that are not weapons in a traditional sense, may benefit from
inherit this module.

A small paragraph about Identifiers
===================================
The Identifiers (or IDs) of objects help the players refer to them in the world.

   |  get shovel
   |  get hat
   |  get sombrero
   |  get white cat
   |  get small cat

Different objects may be referred to by many names, the hat may actually be a sombrero,
but the cat might also be both small and white. The combinations may be endless, imagine
a small white friendly cat in your game? Which IDs should it have?

   |  small white friendly cat
   |  white friendly cat
   |  white cat
   |  cat
   |  small friendly cat
   |  small cat

This quickly explodes into a range of IDs as you can see, so LIMA has a system called
adjectives on top of IDs. For the cat we could handle this as:

.. code-block:: c
   :linenos:

   set_id("cat");
   add_adj("small","white","cat");
   set_in_room_desc("A small white cat is sitting here.");

This would allow the player to refer to the cat by all these adjectives without us
having to make a complete list of combinations of adjectives. This system works for
most objects in LIMA, including weapons.

.. note::

   Rooms have two IDs as well: ``environment`` and ``here`` which allows players
   to ``look at environment`` e.g., but also a wizard to ``update here`` to update
   the room to a newer version as previously discussed.

Melee weapons
=============
Weapons follow the same mechanics of room, in that they use ``setup()`` to define 
name, attributes and so on for the weapon. Let us look at ``^std/weapon/sword.c``:

.. code-block:: c 
   :linenos:

   /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit SWORD;

   void setup()
   {
      set_adj("dull");
      set_weapon_class(5);
      set_weight(1.1);
      set_value(15);
      add_combat_message("miss", "$N $vtake a clumsy swipe at $t, but only $vsucceed in making a fool of $r.");
   }

This is a relatively simple weapon, that still contains a bit of custom functionality. 

Line by line:
   - Line 3: We inherit SWORD (``/std/sword.c``) - go back to the 
     :doc:`basic LIMA guide Section 1.3 <../basic_lpc>`. 
     if you find this puzzling. This is a specialization of WEAPON. If you have many weapons of the 
     same type, this might be a pattern you should follow.
   - Line 5: Our old friend the setup function.
   - Line 7: Adds the adjective "dull" to the sword. It's not very bright.
   - Line 8: This sets how hard the weapon hits. With a weapon class of 5 the weapon will damage opponents
     between 0-5 points, plus damage from strength. Dual wielded weapons may do 1.5 times strength bonus.
   - Line 9: Set the weight of the item in kilos.
   - Line 10: Set the value to 15 (bananas - more on MONEY_D later)
   - Line 11: Sets a special combat message for when you miss with this weapon - more on
     :doc:`messages <Messaging>` here.

**Exercise 1**

   Read ``SWORD``, and try to make sense of the functions used in there. Use ``apropos`` and ``man`` 
   to find out more about the functions. Did you find missing documentation?

.. tip::

   Wait, you don't want to set the weight in kilos? Then change ``#define METRIC`` to
   ``#undef METRIC`` in config.h.

Here is another example with a few more features, a great axe that includes skill restrictions and custom
details for salvaging! More details below.

.. code-block:: c 
   :linenos:

   /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit WEAPON;

   void setup()
   {
      set_id("greataxe", "axe");
      set_weight(3.2);
      set_value(30);
      set_weapon_class(12);
      set_combat_messages("combat-sword");
      set_damage_type("slashing");
      set_skill_used("combat/melee/blade");
      set_skill_restriction("combat/axe", 1);
      set_skill_restriction_message("The greataxe feels foreign in $p hand. $N $vwield it like $n would wield a pickaxe.");
      set_must_dual_wield(1);
      set_salvageable((["wood":15, "metal":85, ]));
   }

Line by line:
   - Line 7: Set the IDs that the weapon will be known by. This line will ensure that the user can
     both use ``wield axe`` and ``wield greataxe``.
   - Line 11: Sets the combat messages the weapon uses (more on messages later, for now look inside the directory
     called ``/data/messages/`` this folder contains standard messages for a lot of things.)
   - Line 13: Here, we set the skill trained by using this weapon.
   - Line 14: This line introduces a skill restriction, saying we need at least to be rank 1 in ``combat/axe``
     to get full benefit of the axe. The player can still use the weapon, but will get told that it's not
     optimal, and will be attacking at reduced efficiency and will do reduced damage.
   - Line 15: The message for a player who does not fulfil the required ranks.
   - Line 16: Not only do we say this this weapon can be dual-wielded here, we say that it must be. Some
     weapons can be wielded in one or two hands, adding more damage should the player want to do so.
   - Line 17: This line tells the ``salvage`` verb what the weapon is made of (more on salvaging and materials later).

**Exercise 2**

   Create your own weapon that uses some of these functions, for now try not to add new damage
   types unless you feel confident doing so. Override some custom messages and experiment with wielding.

.. tip::

   Your newly created weapon should go in ``^pinto/weapon/`` with an appropriate filename. For the rest
   of this guide we will refer to ``^pinto/weapon/flail.c`` but you do not have to create a flail.

.. tip::

   If you want someone to try out your new weapon on, ``^std/monster/flea`` and ``^std/monster/troll``
   would like to help you out. Use :doc:`clone command <../command/clone>` to clone the monsters, and
   :doc:`hp <../player_command/hp>` to check how your opponents are doing.

.. note::

   The :doc:`damage_d <daemon/daemons-damage_d>` keeps track of special attacks for weapons as well, think 
   ``murdering longsword of lightning bolts`` and you get the right picture.



