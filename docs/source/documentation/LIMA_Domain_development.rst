

***********************
LIMA Domain Development
***********************

Prerequisites
#############

To fully benefit from this guide, you should have a basic understanding of LPC and how 
LPMUDs function. This foundational knowledge can be acquired by following the 
:doc:`LPC Basics learning path <Basic_LIMA_Guide>`. By this point, you should be familiar with variables, 
function declarations, function calls, objects, control flows, loops, some basic Linux commands 
that are helpful for working with the MUD, and the file structure used.

Help Improve This Document
--------------------------
This guide is designed for the latest features of the FluffOS driver and LIMA Mudlib.

If you notice any errors or omissions, please submit an issue at https://github.com/tsathoqqua/limadocs/. 
Describe the problem you found or suggest an addition. Even better, feel free to submit a pull request 
with your proposed changes.

1.1 Introduction
================

This document is divided into several sections, and it is recommended that you read them in the order 
they are presented. However, they can also serve as a reference if you need a refresher on a particular 
topic later on. The chapters will build on a continuous example, that you will build in the new domain
*pinto* (also a bean). 

The ``/domains/std`` directory contains many other practical examples of rooms, items, and monsters, 
which you can refer to for learning about features not directly covered in this guide. You will be going
through a series of practical examples, and there will be a tip box in each section pointing giving
direct tips on how to accomplish what you are being asked to do. Try to figure out the exercise yourself
first, before turning to the tip box.

.. note::

    *Armour, armor, colour, colour, categorisation, categorization?*

    LIMA was a mixture of US English and UK English in the 1990s, but has since then been straightened
    out to be fully UK English. This means you can trust that all armour functions, e.g., will be
    always called ``query_armour()`` and not suddenly something else.

    If you want your MUD to be US English, change the interface, i.e., change the 
    :doc:`score <../player_command/score>` and not the mudlib itself. 

2 Creating a Domain
===================
Domains are a mechanism that simplifies the assignment of privileges to wizards and the 
protection of directories.

Since domain creation and membership are strictly controlled using the :doc:`admtool <../command/admtool>`, 
domains and their members can be trusted in all contexts. This trust is utilized in specific parts of 
the mudlib to categorize wizards and determine the commands and options available to them.

Each domain has a unique name, a set of "domain lords," and a group of members. Typically, each domain 
also has an associated directory, such as ``/domains/name/``. The domain name must be composed of lowercase, 
alphabetic characters. Domain lords are wizards with the authority to grant or revoke domain membership 
and have control privileges over the domain. Domain members, on the other hand, have data privileges 
within the domain.

**Exercise 1**:
   
   Let us create the *pinto* domain, now. Use the admtool to do this.

If you do not know how to do this, use the tip box found below.

.. tip::

    1. Open :doc:`admtool <../command/admtool>`.
    2. Goto the privileged menu '1'
    3. Goto domains edit
    4. Create the pinto domain, 'c pinto'.
    5. Verify the domain creating succeeded by going to ``/domains`` and list the files there.

A number of directories have been created inside ``/domains/pinto/``:

  * Armour: Contains armour pieces like hats, gloves, and boots. These items can be worn, with or without 
    providing protective effects.
  * Consumable: Includes items that can be consumed, such as food, drinks, drugs, healing items, and poisons.
  * Item: Holds quest items and other items that players can pick up, often saved with the player's inventory.
  * Mob: Contains mobile entities, usually monsters that can be fought or that move around the area.
  * Npc: Houses non-player characters like quest givers, shopkeepers, and other beings that typically aren't 
    engaged in combat.
  * Obj: Includes objects that are placed in rooms, such as furniture or quest items that cannot be 
    picked up by players.
  * Room: Used for creating areas, structured as a series of interconnected rooms.
  * Weapon: Contains various types of weapons, such as swords, grenades, clubs, pistols, and rocks.

Throughout this guide, we will write code for each of these folders.

2.1 Assigning Roles
-------------------
In a real MUD development environment, you would use the :doc:`admtool <../command/admtool>` to assign 
users and designate a domain lord (lead developer) for each domain. While you can experiment with this to 
test the functionality, it is not necessary for the purposes of this guide.

You can also use the 'show' command to display who has been assigned specific roles within a domain.

.. note::

   An admin is simply a wizard who is part of the admin domain. You can make a wizard an admin 
   by adding them to the admin domain using the admtool. Domain management options can be found 
   under option '1' (admin privilege), then select "Domain".

.. warning::

    Do not delete any domains that come pre-installed with LIMA. Some of these are integral to the 
    security system, and removing them could cause LIMA to malfunction.

2.2 Domain file paths
---------------------
In Linux a home directory has a shortcut that is commonly used, tilde '~', so a ``~tsath`` refers
to ``/home/tsath`` in Linux, and typically ``/wiz/tsath/`` in LIMA. Domains provide another
shortcut to refer to them however, as demonstrated by these shell commands:

     |  /tmp/> cd ^pinto
     |  New cwd: /domains/pinto
     |  ^std/> 

The caret, ``^`` symbol, is a shortcut for ``/domains/`` and it is possible to use in a wide range of
places instead of typing the full path. This shortcut will come in handy when we start creating rooms,
and things in them in Chapter 2. Most places in LIMA, you can use relative paths as well as full paths
which is highly recommended as it provides code that is easier to move around.

.. note::
    
    The ``new()`` function is one of the few places that cannot handle this shortcut (yet).

2.3 Domain #defines
-------------------

.. note::

    If you are new to programming LPMUDs you might be able to skip this section. It addresses
    a bad behaviour used in some old mudlibs.

In some classic LPMUDs it was best practice for area coders to create an include file like this one:

.. code-block:: c

   //Some general shortcuts
   #define TB     this_body()
   #define TO     this_object()

   //Pinto shortcuts
   #define PINTO     "/domains/pinto" 
   #define PIN_ROOM  PINTO "/room/"
   #define PIN_ITEMS PINTO "/items/"
   #define PIN_OBJ   PINTO "/obj/"
   #define PIN_QUEST PINTO "/npc/questgiver"
   ...

Even if this looks smart, and defines can be very helpful for good coding, it is important
to realize when they are being abused.  You *can* have too many ``#define`` statements, and 
you CAN #define the wrong things. Why is this? Basically, macros are difficult to follow when 
there are a lot of them.  This is especially true from people trying to use your
code to help learn.  Seeing ``TB`` as an abbreviation for ``this_body()`` is
not uncommon, and is almost always seen out of context.  For people who
don't quite understand macros, it is VERY confusing to see ``TP``, and trying to figure
out what it means.  Also, even the informed reader may not find your macro
immediately obvious, even if you do, and would likely prefer not to have to search through 
your files to resolve every definition.

The general rule of thumb for using macros is, use them when they represent a variable that 
is subject to change/configuration. Do not use them to abbreviate just because you're too lazy 
to do the typing. This provides easy insight into which variables are built into the objects
by defining them at the top.

An example of some better defines from the source for the :doc:`locate <../command/locate>` command:

.. code-block:: c

   //Iterations we do per call is hardcoded here
   #define ITERS_PER_CALL 50
   //These file paths are hardcoded, so easy to spot
   #define DATA_FILE "/data/locate.codes"
   #define TMP_DATA_FILE "/data/locate.tmp"

So, in conclusion: Do not make include files for domains, they're considered bad style and creates
less readable code that is harder to maintain.


3 Creating rooms
================
Time to create our first room! The first things to decide is which type of room you want to create:

   1. An outdoor room - OUTDOOR_ROOM
   2. An indoor room - INDOOR_ROOM
   3. A room filled with water - WATER_ROOM


3.1 A simple room
-----------------
For now, let us try to create an OUTDOOR_ROOM where the player might encounter weather and other
conditions. Starting simple:

.. code-block:: c
   :linenos:

   inherit OUTDOOR_ROOM;

   void setup()
   {
      set_brief("Murky Swamp");
      set_long("You find yourself in a murky, dank swamp. The air is thick with humidity "
               "and the smell of decaying vegetation. Twisted trees rise from the muddy "
               "water, their gnarled branches reaching towards the dim sky. Patches of "
               "sickly green algae float on the surface of the stagnant pools. "
               "The occasional croak of a frog or buzz of an insect breaks the eerie "
               "silence.");
   }

So what is in here:

   - Line 1, the file we inherit to build upon.
   - Line 3, the ``setup()`` function used in LIMA to initiate the object.
   - Line 5, the brief description of the room. This is shown above the long description.
   - Line 6-11, the long description of the room broken into lines for easy readability.

**Exercise 1**

   Write the code, save this file as `/domains/pinto/room/swamp1.c`, then try to update it to check for 
   issues. Finally, :doc:`goto <../command/goto>` the room. 

.. tip::

   |  /std> cd ^pinto/room
   |  New cwd: /domains/std
   |  ^pinto/room/> update swamp1
   |  /domains/pinto/room/swamp1.c: Updated and loaded.
   |  ^pinto/room/> goto swamp1

Did it work? Otherwise review the error messages the :doc:`update <../command/update>` command 
gives you. The code presented above can be copy pasted using the icon next to it.

3.1 Adding smell and sound
--------------------------

Let us add two more functions to set the smell and a default message for when people listen.

.. code-block:: c
   :linenos:

   inherit OUTDOOR_ROOM;

   void setup()
   {
      set_brief("Murky Swamp");
      set_long("You find yourself in a murky, dank swamp. The air is thick with humidity "
               "and the smell of decaying vegetation. Twisted trees rise from the muddy "
               "water, their gnarled branches reaching towards the dim sky. Patches of "
               "sickly green algae float on the surface of the stagnant pools. "
               "The occasional croak of a frog or buzz of an insect breaks the eerie "
               "silence.");
      set_listen("You hear the occasional croak of frogs and the buzzing of insects.");
      set_smell("The air is thick with the smell of decay and stagnant water.");
   }

New additions:

   - Line 12, set a listen description if someone uses the 'listen' verb (`/cmds/verbs/listen.c`)
   - Line 13, set a smell description if someone uses the 'smell' verb (`/cmds/verbs/smell.c`)

**Exercise 2**

   Update the room, go there, then use ``listen`` and ``smell`` to test the descriptions.
   
.. note::

   If you end up in Void at some point due to an error use the :doc:`wizz <../command/wizz>` command
   to go to the Wizard Lounge, then update the room and then :doc:`goto <../command/goto>` to go
   back to the swamp room.

3.3 Adding details
------------------
Studying the description of the room it talks about trees, water, algae, sky, frog, insects, and mud.
But doing ``look at water`` gives you:

    That doesn't seem to be possible.

We need to add items to the room so we can detail some of these objects further. This is done via 
the ``add_item()`` function defined in rooms. Let us add some items to the room:

.. code-block:: c
   :linenos:

   add_item("trees", "The trees are twisted and gnarled, their roots submerged in "
                     "the murky water. Their branches seem to reach out like "
                     "skeletal fingers.");
   add_item("water", "The water is dark and murky. You can see patches of algae "
                     "floating on its surface.");
   add_item("algae", "Sickly green patches of algae float lazily on the surface "
                     "of the water.");
   add_item("sky", "The sky is barely visible through the canopy of twisted "
                   "branches overhead. What little you can see "
                   "looks gloomy and overcast.");
   add_item("frog", "You don't see any frogs at the moment, but you can hear "
                    "them croaking nearby.");
   add_item("insect", "insects", "bugs",
            "Tiny insects buzz around, occasionally "
            "landing on the surface "
            "of the water or on patches of vegetation.");
   add_item("mud", "The ground is a thick, sticky mud that seems eager "
                   "to pull at your feet.");

As seen above, the syntax is relatively simple:

.. code-block:: c

   add_item(item, description);
   add_item(list of items, description); //See insects in line 13-16 above.

But what if someone wants to pick up the mud, e.g.?

  |  ^pinto/room/> get mud
  |  You can't get that.

That's true, but perhaps we want a more fun description? The ``add_item()`` 
function provides more abilities, let us use those:

.. code-block:: c
   :linenos:

   add_item("mud", (["look":"The ground is a thick, sticky mud that seems eager "
                            "to pull at your feet.",
                       "get":"Your stick your hands in the mud, look at them, "
                             "then decide there are better MUDs.",
                    "search":"You found some dirty hands."]));

As you can see, ``add_item()`` can handle a mapping with different extra options.

**Exercise 3**

   Update the "mud" item to use a mapping like above, and try the different new
   options.

.. note::

   You can use ``update here`` if you are standing in a room you want to update.
   If your code has issues, you will be moved to The Void and will have to go
   back to the room once it loads again.


3.4 Verbs and rooms
-------------------

So what do the verbs actually do? Let us look at the listen verb (`/cmds/verbs/listen.c`).

.. code-block:: c
   :linenos:

   /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit VERB_OB;

   void do_listen_to_obj_with_obj(object ob1, object ob2)
   {
      ob2->do_listen(ob1);
   }

   void do_listen_to_obj(object ob)
   {
      ob->do_listen();
   }

   void do_listen()
   {
      environment(this_body())->do_listen();
   }

   void create()
   {
      add_rules(({"", "to OBJ", "to OBJ with OBJ"}));
   }

The ``create()`` statement at the bottom tells us how we can listen. The rules say we can do:

   1. listen
   2. listen to OBJ
   3. listen to OBJ with OBJ

So, ``listen`` would be rule 1, ``listen to door`` would be rule 2, 
``listen to body with stethoscope`` would be rule 3. The verb *centrally* defines how players
can interact with your MUD. If you want to extend the ways players can interact, you change the verb.
To verify our action, try to modify your the listen command you did in the swamp room earlier to:

    | ``parse listen``

This should us how the parser built into FluffOS tries to discover what to call, when the player
types ``listen``:

   |  /cmds/verbs/> parse listen
   |  Trying interpretation: listen:
   |  Trying rule: 
   |    parse_rule
   |      we_are_finished
   |      Trying can_listen ... (/std/race/documentation#327)
   |      Trying can_listen ... (/std/race/documentation#327)
   |      Trying can_verb ... (/std/race/documentation#327)
   |      Trying can_verb_rule ... (/std/race/documentation#327)
   |      Trying can_listen ... (/cmds/verbs/listen)
   |      Trying can_listen ... (/cmds/verbs/listen)
   |      Trying can_verb ... (/cmds/verbs/listen)
   |      Trying can_verb_rule ... (/cmds/verbs/listen)
   |      Return value was: 1
   |      Saving successful match: do_listen (cmds/verbs/listen)
   |    exiting parse_rule ...
   |  Calling do_listen ...
   |  You hear nothing unusual.
   |  1

The lines after 'we_are_finished' shows the parser probing ``/std/race/documentation#327`` which
is the body of the player calling the parse command, and getting probed for ``can_listen()``, then
same probe in the verb object, then finally falling back to calling ``can_verb_rule()``.
This function is called with the following arguments:

.. code-block:: c

   verb->can_verb_rule();

This function runs some checks required by the verb, like do we need to be able to see, do we 
need to be alive to do the action. If these checks pass, it returns 1. 

.. note::

   You can use the wizard shell to check what the verb returns like this:

   ``@./cmds/verbs/listen->can_verb_rule()``

.. note::
   
   An example of the default checks for verbs is the drop verb that has the following
   code, to allow people to drop things even in the darkness:

   .. code-block:: c

         clear_flag(NEED_TO_SEE);


At the end of the parse, the parser decides there is a successful match for the rule, and decides
to call ``do_listen()`` in the verb, this is shown in line 15-18 above. Let us look closer at that
specific code in line 17:

.. code-block:: c

   environment(this_body())->do_listen();

So, find the environment of ``this_body()``, that would be the room the player is standing in
meaning ``swamp1.c``, and then call ``do_listen()`` in the room! So if we wanted more advanced
functionality we have just learned that the verb calls a built-in function we inherited, but
we could also override it and return something random like:

.. code-block:: c

      "You hear "+(random(3)+2)+" animals fighting in the distance"

``random(3)`` returns a number between 0-2 plus 2, so that would turn into 2-4 animals.

**Exercise 4**

   Add a new function called ``do_listen()`` to ``swamp1.c`` that uses ``write()`` to send
   the string with the fighting animals above to the current user. 
   Remember, that the function declaration should state that the function takes no parameters, and returns
   nothing.

After you are done updating the room, update it, go to it, and do several 'listen' to test
the new functionality.

.. tip::

    The function that should be added should look like this:

    .. code-block:: c
    
       void do_listen() { write("...."); }

How would we know? You can use :doc:`dbxfuncs <../command/dbxfuncs>` to list the functions
in OUTDOOR_ROOM, like this:

   |  ^pinto/room/> dbxfuncs /std/outdoor_room do_
   |  Matches:
   |  int do_verb_rule(x, x, x)     (defined in /std/object/vsupport)
   |  void do_search(x, x)          (defined in /std/object/vsupport)
   |  int do_not_restore()          (defined in /std/object)
   |  void do_receive(x, x)         (defined in /std/container)
   |  void do_go_str(x)             (defined in /std/modules/m_exit)
   |  void do_looking(x, x)         (defined in /std/room/roomdesc)
   |  void do_pray()                (defined in /std/base_room)
   |  **void do_listen()              (defined in /std/base_room)**
   |  void do_look_at_str()         (defined in /std/base_room)
   |  void do_smell()               (defined in /std/base_room)
   |  ^pinto/room/>

An other option would be to use the :doc:`apropos <../command/apropos>` command to look
for documented ``do_listen()`` functions:

   |  ^pinto/room/> apropos do_listen
   |  [autodoc/mudlib]:  do_listen
   |  ^pinto/room/>

This tells us, that the function has been documented via autodoc in the mudlib section. 
Using ``man do_listen`` or ``help do_listen`` will show the help page this function is
documented on. You will get the entire context of the function as well which can lead
to new ideas on which functions to call. You would get this page in game: 

   :doc:`Mudlib base_room <../mudlib/std-base_room>` 

since the function is documented here.

**Exercise 5**

   Use the `::` operator to call the original ``do_listen()`` function that you have
   overwritten in your current code, so they are both your new function and the one defined
   in OUTDOOR_ROOM is called.


.. tip::

   Here is the final code for Exercise 6, if you are having issues:

   .. code-block:: c
   
       inherit OUTDOOR_ROOM;

      void setup()
      {
         set_brief("Murky Swamp");
         set_long("You find yourself in a murky, dank swamp. The air is thick with humidity "
                  "and the smell of decaying vegetation. Twisted trees rise from the muddy "
                  "water, their gnarled branches reaching towards the dim sky. Patches of "
                  "sickly green algae float on the surface of the stagnant pools. "
                  "The occasional croak of a frog or buzz of an insect breaks the eerie "
                  "silence.");
         set_listen("You hear the occasional croak of frogs and the buzzing of insects.");
         set_smell("The air is thick with the smell of decay and stagnant water.");
         add_item("trees", "The trees are twisted and gnarled, their roots submerged in "
                  "the murky water. Their branches seem to reach out like "
                  "skeletal fingers.");
         add_item("water", "The water is dark and murky. You can see patches of algae "
                  "floating on its surface.");
         add_item("algae", "Sickly green patches of algae float lazily on the surface "
                  "of the water.");
         add_item("sky", "The sky is barely visible through the canopy of twisted "
                  "branches overhead. What little you can see "
                  "looks gloomy and overcast.");
         add_item("frog", "You don't see any frogs at the moment, but you can hear "
                  "them croaking nearby.");
         add_item("insect", "insects", "bugs",
                  "Tiny insects buzz around, occasionally "
                  "landing on the surface "
                  "of the water or on patches of vegetation.");
         add_item("mud", (["look":"The ground is a thick, sticky mud that seems eager "
                                  "to pull at your feet.",
                            "get":"Your stick your hands in the mud, look at them, "
                                  "then decide there are better MUDs.",
                         "search":"You found some dirty hands."]));
      }

      void do_listen()
      {
         ::do_listen();
         write("You hear " + (random(3) + 2) + " animals fighting in the distance");
      }

4 Creating weapons
==================
Weapons in LIMA all inherit ``M_DAMAGE_SOURCE``, meaning something that can be a source
of damage. So even things that are not weapons in a traditional sense, may benefit from
inherit this module.

4.1 A small paragraph about Identifiers
---------------------------------------
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

4.2 Melee weapons
-----------------
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
     :doc:`basic LIMA guide Section 1.3 <Basic_LIMA_Guide>`. 
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


5 Creating armour
=================

Weapons are using ``M_DAMAGE_SOURCE`` and armours are using ``M_DAMAGE_SINK``, as damage is transferred
from the source to the sink. If ``<config.h>`` has ``#define USE_DURABILITY`` damage inflicted from
a weapon is directly reduced from the the sink (it has to go somewhere if it doesn't hit the player, right?).

5.1 A simple armour
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


5.2 Limbs and body types (briefly)
----------------------------------
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

5.2 Armours with more pieces
----------------------------
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
   of the "Functions" chapter in :doc:`LPC Basics learning path <Basic_LIMA_Guide>` 
   (Don't click unless you have no idea).

   Now, that you know the bullet number, can you explain why we do not need armour class for gloves?

5.3 Resistances and weaknesses
------------------------------

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
     :doc:`in Section 3.2 <LIMA_Domain_development_c3#melee-weapons>`.
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


6 Creating NPCs
===============

**UNDER CONSTRUCTION**

6.2 Modules
-----------
The LIMA mudlib consists of more than 60 modules located in ``/std/modules``. These modules all got shorthand
names found in ``/include/mudlib.h`` like many other things. The modules *try* to be independent and modular,
but some still depend on others.

Ideally, the modules provides functionality that you can add to your code as needed. Need another feature?
Inherit another module. Here is the top most lines from ``^std/monster/robert.c``:

.. code-block:: c 

   inherit LIVING;       
   inherit M_NPCSCRIPT;
   inherit M_TRIGGERS;   
   inherit M_SMARTMOVE;

As you can see "Bob" inherits ``LIVING`` (a non combat monster), but also ``M_NPCSCRIPT`` that gives Bob
ability to use NPC scripts (more on those later). If you open the ``M_NPCSCRIPT`` file you will see:

.. code-block:: c 

   ...
   inherit M_ACTIONS;

So ``M_NPCSCRIPT`` requires ``M_ACTIONS``, a module that is requires for NPCs to take actions in the world (get, give, 
speak, etc.), but ``M_NPCSCRIPT`` can also do triggers, meaning wait for events in the MUD and act upon them. This
only works if ``M_TRIGGERS`` is also inherited. Finally, ``M_SMARTMOVE`` provides the ability for Bob to move.

All these abilities could be inherited always into ``M_NPCSCRIPT``, but this way gives you, as a programmer,
the option to extend the capabilities of Bob as you see fit, or reduce them to less. In general, we do not want
out MUD objects to inherit more things than what is needed - it is all about keeping the memory and CPU usage 
down to what is needed - the reverse is sometimes referred to as "bloat", "bloated code", or "bloated objects".

A short list of modules typically used for monsters is shown here:

  - :doc:`m_accountant <../module/modules-m_accountant>` - for NPCs running bank departments
  - :doc:`m_actions <../module/modules-m_actions>` - for NPCs that need to be able to take actions
  - :doc:`m_aggressive <../module/modules-m_aggressive>` - for NPCs that should attack on sight
  - :doc:`m_assistance <../module/modules-m_assistance>` - for NPCs that call for assistance
  - :doc:`m_blockexits <../module/modules-m_blockexits>` - for NPCs that should block exits
  - :doc:`m_boss <../module/modules-m_boss>` - for boss monsters
  - :doc:`m_companion <../module/modules-m_companion>` - for companions monsters that can be trained by the players
    and follow them around
  - :doc:`m_conversation <../module/modules-m_conversation>` - for interactive menus that allow players to talk to
    the NPCs in a RPG style chat, where one option can add or remove new options and give quests based on stats
    and skills.
  - :doc:`m_follow <../module/modules-m_follow>` - for monsters that should follow if the player leaves.
  - :doc:`m_npcscript <../module/modules-m_npcscript>` - for scripting sequences of steps for NPCs
  - :doc:`m_smartmove <../module/modules-m_smartmove>` - for monsters that should be able to move
  - :doc:`m_trainer <../module/modules-m_trainer>` - for NPCs that should be able to train skills or stats
  - :doc:`m_vendor <../module/modules-m_vendor>` - for NPCs that sell and buy stuff (shopkeepers, e.g.)
  - :doc:`m_wander <../module/modules-m_wander>` - for monsters that should wander around randomly

.. info::

    There is ongoing work with modules making them more independent. Earlier, they had hidden dependencies, e.g.
    module A depending on module B, but this has been changed to module A now inheriting module B so you do not
    have to figure out these dependencies. This does, however, mean that if you already inherit module B, and then
    start inheriting module A, you should no longer inherit module B in your code, as it is included in module A.


6.1 Banking NPCs
----------------

.. code-block:: c 
   :linenos:

    /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit ADVERSARY;
   inherit M_ACCOUNTANT;

   void setup()
   {
      set_name("Samuel");
      add_id("accountant", "sam");
      set_gender(1);
      set_proper_name("Samuel the Bank Accountant");
      set_in_room_desc("Samuel the Bank Accountant stands behind the counter.");
      set_long("Samuel is a boring looking balding man. Perfectly clothed of "
               "course.");
      set_bank_id("Bean");
      set_bank_name("The Imperial Bank of the Bean");
      set_currency_type("gold");
      set_exchange_fee(5);
   }
