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

.. disqus::
