CHAPTER 4: Creating armour
==========================

**THIS ARTICLE IS INCOMPLETE**

Weapons are using ``M_DAMAGE_SOURCE`` and armours are using ``M_DAMAGE_SINK``, as damage is transferred
from the source to the sink. If ``<config.h>`` has ``#define USE_DURABILITY`` damage inflicted from
a weapon is directly reduced from the the sink (it has to go somewhere if it doesn't hit the player, right?).

4.1 A simple armour
-------------------
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


4.2 Limbs and body types (briefly)
----------------------------------
Body types, like "humanoid", "insect", "quadruped", are defined in the BODY_D. This daemon is 
best administered through the `admtool <../command/admtool.html>`_. By this time, you should
have tried out the tool, before.

**Exercise 1**

   Use the admtool to list the body types that exist in LIMA right now, and print the limbs
   of a griffin.

Let us look at a humanoid body type:

   |  Limb                 Health          Parent Limb     Type
   |  --------------------------------------------------------------------
   |  head                 20              torso           vital
   |  right leg            15              torso           mobile
   |  right foot           -1              right leg
   |  neck                 -1              torso
   |  back                 -1              torso
   |  left arm             15              torso           wielding
   |  left foot            -1              left leg
   |  left wrist           -1              left arm
   |  left leg             15              torso           mobile
   |  torso                20                              vital
   |  right wrist          -1              right arm
   |  right hand           -1              right wrist
   |  right arm            15              torso           wielding
   |  left hand            -1              left wrist

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

.. disqus::
