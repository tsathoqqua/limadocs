***********************
LIMA Domain Development
***********************

Prerequisites
-------------
To fully benefit from this guide, you should have a basic understanding of LPC and how 
LPMUDs function. This foundational knowledge can be acquired by following the 
`LPC Basics learning path <Basic_LIMA_Guide.html>`_. By this point, you should be familiar with variables, 
function declarations, function calls, objects, control flows, loops, some basic Linux commands 
that are helpful for working with the MUD, and the file structure used.

Help Improve This Document
--------------------------
This guide is designed for the latest features of the FluffOS driver and LIMA Mudlib.

If you notice any errors or omissions, please submit an issue at https://github.com/tsathoqqua/limadocs/. 
Describe the problem you found or suggest an addition. Even better, feel free to submit a pull request 
with your proposed changes.

Introduction
============
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
    `score <../player_command/score.html>`_ and not the mudlib itself. 


CHAPTER 1: Creating a Domain
============================
Domains are a mechanism that simplifies the assignment of privileges to wizards and the 
protection of directories.

Since domain creation and membership are strictly controlled using the `admtool <../command/admtool>`_, 
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

    1. Open `admtool <../command/admtool.html>`_.
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

1.1 Assigning Roles
-------------------
In a real MUD development environment, you would use the `admtool <../command/admtool.html>`_ to assign 
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

1.2 Domain file paths
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

1.3 Domain #defines
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

An example of some better defines from the source for the `locate <../command/locate.html>`_ command:

.. code-block:: c

   //Iterations we do per call is hardcoded here
   #define ITERS_PER_CALL 50
   //These file paths are hardcoded, so easy to spot
   #define DATA_FILE "/data/locate.codes"
   #define TMP_DATA_FILE "/data/locate.tmp"

So, in conclusion: Do not make include files for domains, they're considered bad style and creates
less readable code that is harder to maintain.

CHAPTER 2: Creating rooms
==========================
Time to create our first room! The first things to decide is which type of room you want to create:

   1. An outdoor room - OUTDOOR_ROOM
   2. An indoor room - INDOOR_ROOM
   3. A room filled with water - WATER_ROOM


2.1 A simple room
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

**Exercise 2**

   Write the code, save this file as `/domains/pinto/room/swamp1.c`, then try to update it to check for 
   issues. Finally, `goto <../command/goto.html>`_ the room. 

.. tip::

   |  /std> cd ^pinto/room
   |  New cwd: /domains/std
   |  ^pinto/room/> update swamp1
   |  /domains/pinto/room/swamp1.c: Updated and loaded.
   |  ^pinto/room/> goto swamp1

Did it work? Otherwise review the error messages the `update <../command/update.html>`_ command 
gives you. The code presented above can be copy pasted using the icon next to it.

2.1 Adding smell and sound
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

**Exercise 3**

   Update the room, go there, then use ``listen`` and ``smell`` to test the descriptions.
   
.. note::

   If you end up in Void at some point due to an error use the `wizz <../command/wizz.html>`_ command
   to go to the Wizard Lounge, then update the room and then `goto <../command/goto.html>`_ to go
   back to the swamp room.

2.3 Adding details
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

**Exercise 4**

   Update the "mud" item to use a mapping like above, and try the different new
   options.

.. note::

   You can use ``update here`` if you are standing in a room you want to update.
   If your code has issues, you will be moved to The Void and will have to go
   back to the room once it loads again.


2.4 Verbs and rooms
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

   |  /cmds/verbs/>parse listen
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


At the end of the parse, the parser decides there is a successfull match for the rule, and decides
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

**Exercise 5**

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

How would we know? You can use `dbxfuncs <../command/dbxfuncs.html>`_ to list the functions
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

An other option would be to use the `apropos <../command/apropos.html>`_ command to look
for documented ``do_listen()`` functions:

   |  ^pinto/room/> apropos do_listen
   |  [autodoc/mudlib]:  do_listen
   |  ^pinto/room/>

This tells us, that the function has been documented via autodoc in the mudlib section. 
Using ``man do_listen`` or ``help do_listen`` will show the help page this function is
documented on. You will get the entire context of the function as well which can lead
to new ideas on which functions to call. You would get this page in game: 

   `Mudlib base_room <../mudlib/std-base_room.html>`_ 

since the function is documented here.

**Exercise 6**

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

CHAPTER 3: Creating weapons
===========================
Weapons follow the same mechanics of room, in that they use ``setup()`` to define 
name, attributes and so on for the weapon. Let us look at ``^std/weapon/greataxe.c``:

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
   - Line 3: We inherit WEAPON (``/std/weapon.c``) - go back to the 
     `basic LIMA guide Section 1.3 <Basic_LIMA_Guide.html#shortcuts-for-filenames>`_ 
     if you find this puzzling.
   - Line 5: Our old friend the setup function.
   - Line 7: Set the IDs that the weapon will be known by. This line will ensure that the user can
     both use ``wield axe`` and ``wield greataxe``.
   - Line 8: Set the weight of the item in kilos.
   - Line 9: Set the value to 30 (something - more on MONEY_D later)
   - Line 10: This sets how hard the weapon hits. With a weapon class of 12 the weapon will damage opponents
     between 0-11 points, plus damage from strength. Dual wielded weapons may do 1.5 times strength bonus.
   - Line 11: Sets the combat messages the weapon uses (more on messages later, for now look inside the directory
     called ``/data/messages/`` this folder contains standard messages for a lot of things.)
   - Line 12: This sets the damage type of the weapon. Damage types are define in the 
     `DAMAGE_D <../daemon/daemons-damage_d.html>_`
   - Line 13: Here, we set the skill trained by using this weapon.
   - Line 14: This line introduces a skill restriction, saying we need at least to be rank 1 in ``combat/axe``
     to get full benefit of the axe. The player can still use the weapon, but will get told that it's not
     optimal, and will be attacking at reduced efficiency and will do reduced damage.
   - Line 15: The message for a player who does not fulfil the required ranks - there is a 
     `lot more be said about messages <Messaging.html>`_.
   - Line 16: Not only do we say this this weapon can be dual-wielded here, we say that it must be. Some
     weapons can be wielded in one or two hands, adding more damage should the player want to do so.
   - Line 17: This line tells the ``salvage`` verb what the weapon is made of (more on salvaging and materials later).

So, a lot of similarities to room, ``setup()``, calls to lots of functions to add features to the object.

.. tip::

   Wait, you don't want to set the weight in kilos? Then change ``#define METRIC`` to
   ``#undef METRIC`` in config.h.

.. note::

   The DAMAGE_D keeps track of special attacks for weapons as well, think ``murdering longsword of lightning bolts``
   and you get the right picture.