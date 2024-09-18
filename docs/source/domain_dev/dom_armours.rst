###############
Creating armour
###############

Weapons are using ``M_DAMAGE_SOURCE`` and armours are using ``M_DAMAGE_SINK``, as damage is transferred
from the source to the sink. If ``<config.h>`` has ``#define USE_DURABILITY`` damage inflicted from
a weapon is directly reduced from the the sink (it has to go somewhere if it doesn't hit the player, right?).

A simple armour
===============
Let us start by looking at ``^std/armour/scarf.c``:

.. code-block:: c
   :linenos:

   inherit ARMOUR;

   void setup()
   {
      set_adj("red");
      set_id("scarf");
      set_slot("head");
      set_value(1);
      set_weight(0.1);
   }

Not much new above compared to weapons, right? Except the ``set_slot("head");`` call on line 7. This call
tells the MUD that this piece of armour goes on the "head" limb. 

.. warning:: 

   LIMA can be set up to not use limbs, but the default examples include limb configurations as this
   is the most complex configuration of the armour system. If you do not want to use limbs modify
   the config in ``<combat_modules.h>``. Modifying these will likely invalidate all your player files.


Limbs and body types (briefly)
==============================
Body types, like "humanoid", "insect", "quadruped", are defined in the BODY_D. This daemon is 
best administered through the :doc:`admtool <../command/admtool>`. By this time, you should
have tried out the tool, before.

**Exercise 1**

   Use the admtool to list the body types that exist in LIMA right now, and print the limbs
   of a griffin.

Let us look at a humanoid body type:

+--------------+---------+-------------+-----------------+
| Limb         | Health  | Parent Limb | Type            |
+==============+=========+=============+=================+
| head         |  20     | torso       | vital           |
+--------------+---------+-------------+-----------------+
| right leg    |  15     | torso       | mobile          |
+--------------+---------+-------------+-----------------+
| right foot   |  -1     | right leg   |                 |
+--------------+---------+-------------+-----------------+
| neck         |  -1     | torso       |                 |
+--------------+---------+-------------+-----------------+
| back         |  -1     | torso       |                 |
+--------------+---------+-------------+-----------------+
| left arm     |  15     | torso       | wielding        |
+--------------+---------+-------------+-----------------+
| left foot    |  -1     | left leg    |                 |
+--------------+---------+-------------+-----------------+
| left wrist   |  -1     | left arm    |                 |
+--------------+---------+-------------+-----------------+
| left leg     |  15     | torso       | mobile          |
+--------------+---------+-------------+-----------------+
| torso        |  20     |             | vital           |
+--------------+---------+-------------+-----------------+
| right wrist  |  -1     | right arm   |                 |
+--------------+---------+-------------+-----------------+
| right hand   |  -1     | right wrist |                 |
+--------------+---------+-------------+-----------------+
| right arm    |  15     | torso       | wielding        |
+--------------+---------+-------------+-----------------+
| left hand    |  -1     | left wrist  |                 |
+--------------+---------+-------------+-----------------+

When creating a body type, a few rules apply:

   1. The total sum of health must be 100, ignoring all the -1 values.
      (Humanoid: 20+15+15+15+20+15=100).
   2. Start with the torso (or center mass of your body).
   3. Add limbs while using your torso as parent, and built from there.
   4. If a limb has -1 health, it can wear armour, but cannot be hit
      during combat.
   5. Limbs come three categories:
      1. Vital: If this limb is reduced to 0 hit points the creature dies.
         "head" is normally always vital.
      2. Mobile: If the creature gets hit hard enough in mobile limbs it
         might fall over.
      3. Wielding: These limbs allow the body to carry weapons. The more
         the merrier!
   6. Don't add too many limbs that do nothing. 

.. note::
   
   The "insect" body type
   is a good example of this. There is no way a player would be hitting
   "leg 3", "leg 4" on a small spider with his longsword. On the Other
   hand, if you got a monstrously big spider it might make the combat
   more interesting to add all 8 legs and 2 fangs.

**Exercise 2**

   Create a "bird" body type, with the limbs you find appropriate.
   Make sure to follow the rules listed above.

Got a body you are happy with? (Otherwise, go to the gym hueh hueh hueh).
Use the following template for a bird (or modify as you please):

.. code-block:: c 
   :linenos:

   /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit ADVERSARY;

   void setup()
   {
      set_name("swan");
      set_id("swan");
      set_in_room_desc("A white swan is standing here.");
      set_combat_messages("combat-claws-bites");
      set_long("A swan");
      update_body_style("bird");
      set_level(10);
   }

.. note: 

   The swan above is using ``combat-claws-bites`` which is obviously not how
   swans fight, but we will live with this for now.

**Exercise 3**

   Create the swan above after adding your body. Clone the swan, use the ``hp``
   command to see if the limbs are as they should be. Try to fight it, and see
   if it dies when the vital limbs are down to 0.

   Optionally: Create a small armour for it that it can wear on each wing.

.. note:

   We will cover monsters in more details, but for now this is good enough to
   create a swan.

Armours with more pieces
========================
Armours do sometimes not just cover one limb, think of a pair of gloves (both hands),
or a jacket (which does not just cover the torso, but also left arm, right arm and the back).

LIMA has a simple system for situations like this, let us look at a pair of gloves:

.. code-block:: c 
   :linenos:

   /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit ARMOUR;

   void setup()
   {
      set_adj("pair of");
      set_armour_class(3);
      set_id("gloves");
      set_long("These are black gloves made of fine leather. Perhaps.");
      set_slot("left hand");
      set_also_covers("right hand");
   }

Most of the things above, you will recognise at this point from the scarf, but in line 12 we have a new
function call ``set_also_covers("right hand");``. This function barely needs any explanation, it tells
the gloves that they cover the right hand as well.

**Exercise 4**

   Why do we not need ``set_armour_class(3);`` in line 8 for the gloves?

.. tip::

   The *answer* to this, is in the list below the table above describing rules for body creation.
   
   To not give you the answer directly, you can find the correct number for above, by finding the number
   of the "Functions" chapter in :doc:`LPC Basics learning path <../basic_lpc/lpc_functions>` 
   (Don't click unless you have no idea).

   Now, that you know the bullet number, can you explain why we do not need armour class for gloves?

Resistances and weaknesses
==========================

Here is another example, a kevlar vest that can be worn underneath another jacket in your game:

.. code-block:: c 
   :linenos:

   /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit ARMOUR;

   void setup()
   {
      set_id("vest");
      add_adj("kevlar", "old");
      set_slot("torso");
      set_long("A old kevlar vest made with a few still functional velcro straps. It provides protection against regular "
               "bullets, but is slightly vulnerable to plasma rounds due to some of the metal bands used inside it.");
      set_armour_class(random(5) + 2);
      set_worn_under(1);
      set_wearmsg("$N $vstrap on a kevlar vest.");
      set_resistances((["force":20]));
      set_weaknesses((["slashing":5]));
      set_salvageable((["textile":60, "metal":40]));
   }

A few more interesting lines here:
   - Line 1-11: You should have seen all these before.
   - Line 12: A small variant of ``set_armour_class()`` where we use a ``random()`` function to give players a reason
     to hunt the best possible kevlar vest (not all vests are created equal), i.e. they have AC from 2-6.
   - Line 13: ``set_worn_under(1)`` tells the limb system that this item can be worn under other items covering that body part.
     Otherwise, the player would receive a ``You discover you cannot wear the kevlar vest.``  when trying to wear the vest
     with the leather jacket (see ``^std/armour/leather_jacket.c``).
   - Line 14: A custom message for when a player wears the vest can be set this way. Otherwise the default ``"$N $vwear a $o."``
     and ``"$N $vremove $p $o."`` messages will be used.
   - Line 15: This is a special leather jacket that will increase the effective armour class against force with 20 points 
     ("force" is a damage type defined in the :doc:`damage_d <../daemon/daemons-damage_d>`).
   - Line 16: Weakness on the other hand decreases the armour class by 5 points of the damage the players receive from this damage type. 
     In this case the kevlar jacket is easy to slash through, but will stop (some) force, i.e. from bullets.
   - Line 17: This defines the materials that can be salvaged, as we saw 
     :doc:`in Section 3.2. Melee weapons <../basic_lpc/dom_weapons>`.
     The amount of materials depends on the weight of the item, as it represents how much there is to salvage. We did not explicitly set
     the weight of the vest here, but you could do that.

.. tip::

   To understand the damage function exactly see the ``class event_info sink_modify_event(class event_info evt)`` function in
   the :doc:`m_damage_source <../module/modules-m_damage_source>` module. Change to fit your needs.

**Exercise 5**

   Create your own armour piece that uses some of the new functions you learned above, set some weaknesses and resistances,
   and test your armour on the test dummy found in ``^std/monster/test_dummy.c``. Clone the dummy, give it your armour, and
   it will automatically wear whatever you give it (or attempt to).

Notice how the message change depending on how hard the dummy is hit.

.. tip::

   Use ``equip dummy`` to monitor what the dummy is wearing, and ``equip`` to see which weapon you are doing damage with, and
   how it is impacting the dummy.
   You will also see your skill ranks going up while doing this, ``skills`` to check.

.. note::

   The dummy will never lose hit points.

.. disqus::

