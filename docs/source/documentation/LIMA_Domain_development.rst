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

1.2 Domain File Paths
---------------------
In Linux, the tilde `~` is commonly used as a shortcut for the home directory. For 
example, ``~tsath`` refers to ``/home/tsath`` in Linux, and typically ``/wiz/tsath/`` in 
LIMA. Similarly, domains in LIMA also have a shortcut, as shown by these shell commands:

     |  /tmp/> cd ^pinto
     |  New cwd: /domains/pinto
     |  ^std/> 

The caret symbol ``^`` acts as a shortcut for ``/domains/``, allowing you to avoid 
typing the full path. This can be used in many places, and it will become useful when we 
start creating rooms and other content in Chapter 2. You can also use relative paths in most 
parts of LIMA, which is recommended as it makes your code more portable and easier to move.

.. note::
    
    The ``new()`` function is one of the few places that cannot handle this shortcut (yet).

1.3 Domain #defines
-------------------

.. note::

    If you’re new to LPMUD programming, you might be able to skip this 
    section. It addresses outdated practices from older mudlibs.

In older LPMUDs, area coders often created include files with shortcuts like this:

.. code-block:: c

   // General shortcuts
   #define TB     this_body()
   #define TO     this_object()

   // Pinto-specific shortcuts
   #define PINTO     "/domains/pinto" 
   #define PIN_ROOM  PINTO "/room/"
   #define PIN_ITEMS PINTO "/items/"
   #define PIN_OBJ   PINTO "/obj/"
   #define PIN_QUEST PINTO "/npc/questgiver"
   ...

While this seems efficient, overusing ``#define`` can lead to confusion and difficult-to-read code. 
Having too many ``#define`` statements, or using them inappropriately, makes the code harder to 
follow—especially for those trying to learn from your work. For instance, abbreviations 
like ``TB`` for ``this_body()`` or ``TP`` can be confusing, even to experienced readers.

The rule of thumb is to use macros when they represent variables that are likely to change 
or need configuration. Avoid using them merely as shortcuts to save typing. This helps make 
it clear which variables are integral to your objects by defining them at the top of your code.

Here’s an example of a better use of defines from the source of the `locate <../command/locate.html>`_ command:

.. code-block:: c

   // Number of iterations per call is defined here
   #define ITERS_PER_CALL 50
   // File paths are hardcoded for easy recognition
   #define DATA_FILE "/data/locate.codes"
   #define TMP_DATA_FILE "/data/locate.tmp"

In conclusion, avoid creating include files for domains. They are considered bad style and make 
the code less readable and harder to maintain.

CHAPTER 2: Creating Rooms
=========================
Now let’s create our first room! First, decide what type of room you want to make:

   1. An outdoor room - OUTDOOR_ROOM
   2. An indoor room - INDOOR_ROOM
   3. A water-filled room - WATER_ROOM

2.1 A Simple Room
-----------------
Let’s start with an OUTDOOR_ROOM where the player might encounter weather and other environmental 
conditions. Here’s a simple example:

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

Explanation of the code:

   - Line 1: We inherit the OUTDOOR_ROOM class to build upon it.
   - Line 3: The ``setup()`` function initializes the object in LIMA.
   - Line 5: ``set_brief()`` sets a short description of the room.
   - Lines 6-11: ``set_long()`` provides a more detailed description, broken into lines for readability.

**Exercise 2**

   Write the code above, save it as `/domains/pinto/room/swamp1.c`, and use the 
   `update <../command/update.html>`_ command to check for issues. Then `goto <../command/goto.html>`_ the room.

.. tip::

   |  /std> cd ^pinto/room
   |  New cwd: /domains/std
   |  ^pinto/room/> update swamp1
   |  /domains/pinto/room/swamp1.c: Updated and loaded.
   |  ^pinto/room/> goto swamp1

If it didn’t work, review the error messages provided by the ``update`` command. You can copy 
and paste the code above using the copy icon.

2.2 Adding Smell and Sound
--------------------------
Now, let’s enhance the room by adding functions to describe its smell and the sounds players might hear:

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

Explanation of the new code:

   - Line 12: Adds a description for when someone uses the 'listen' verb (`/cmds/verbs/listen.c`).
   - Line 13: Adds a description for when someone uses the 'smell' verb (`/cmds/verbs/smell.c`).

**Exercise 3**

   Update the room, go there, and test the ``listen`` and ``smell`` descriptions.

.. note::

   If you end up in the Void due to an error, use the `wizz <../command/wizz.html>`_ command 
   to go to the Wizard Lounge, update the room, and then ``goto`` back to the swamp.

2.3 Adding Details
------------------
The room description mentions trees, water, algae, sky, frogs, insects, and mud, but if 
someone tries to ``look at water``, they’ll see:

    That doesn't seem to be possible.

To make these objects interactable, we need to add them as items using the ``add_item()`` function:

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

As you can see, the syntax is simple:

.. code-block:: c

   add_item(item, description);
   add_item(list of items, description); //See the example for insects in lines 13-16.

But what if someone tries to pick up the mud?

  |  ^pinto/room/> get mud
  |  You can't get that.

We can add a more fun response using ``add_item()`` with additional options:

.. code-block:: c
   :linenos:

   add_item("mud", (["look":"The ground is a thick, sticky mud that seems eager "
                            "to pull at your feet.",
                      "get":"You stick your hands in the mud, then decide there are better MUDs.",
                   "search":"You found some dirty hands."]));

As you can see, ``add_item()`` can handle mappings to provide different responses for various actions.

**Exercise 4**

   Update the "mud" item to use a mapping like the one above, and try the different options.

.. note::

   You can use ``update here`` if you're standing in the room you want to update. If your code has issues, 
   you will be moved to The Void and need to return to the room after fixing the code.


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

Verbs in MUD Systems
====================

In a MUD (Multi-User Dungeon), **verbs** are central to defining how players interact with the world. 
Each verb controls specific player actions. To add more interaction options for players, 
you modify or introduce new verbs.

For example, consider the verb ``listen``:

- **Rule 1**: ``listen``
- **Rule 2**: ``listen to door``
- **Rule 3**: ``listen to body with stethoscope``

The verb *centrally* defines how players interact with your MUD. If you want to expand how players 
interact, modify the verb. You can test how the parser discovers what to call by modifying 
the `listen` command in the swamp room as follows:

.. code-block:: text

    parse listen

This will show how FluffOS processes the player's input and tries to determine which function to 
call when the player types ``listen``. For example:

.. code-block:: text

    /cmds/verbs/> parse listen
    Trying interpretation: listen:
    Trying rule: 
      parse_rule
        we_are_finished
        Trying can_listen ... (/std/race/documentation#327)
        Trying can_listen ... (/std/race/documentation#327)
        Trying can_verb ... (/std/race/documentation#327)
        Trying can_verb_rule ... (/std/race/documentation#327)
        Trying can_listen ... (/cmds/verbs/listen)
        Trying can_listen ... (/cmds/verbs/listen)
        Trying can_verb ... (/cmds/verbs/listen)
        Trying can_verb_rule ... (/cmds/verbs/listen)
        Return value was: 1
        Saving successful match: do_listen (cmds/verbs/listen)
      exiting parse_rule ...
    Calling do_listen ...
    You hear nothing unusual.
    1

In the lines following `we_are_finished`, the parser checks ``/std/race/documentation#327`` 
(the player's body) for the function ``can_listen()``, then looks for it in the verb object, 
and finally uses ``can_verb_rule()``.

The ``can_verb_rule()`` function is called as follows:

.. code-block:: c

    verb->can_verb_rule();

This function checks the necessary conditions for performing the verb. For example, 
the ``listen`` verb may require that the player is alive or conscious. If these checks 
pass, the function returns ``1``.

.. note::

    You can verify the verb's return value using the wizard shell with the following command:

    ``@./cmds/verbs/listen->can_verb_rule()``

After verifying the rule, the parser decides to call ``do_listen()``. The corresponding lines 
in the parser output look like this:

.. code-block:: text

    Calling do_listen ...
    You hear nothing unusual.

Let’s look closer at the code:

.. code-block:: c

    environment(this_body())->do_listen();

Here, the parser is calling the ``do_listen()`` function in the player’s current environment, 
which is the room the player is standing in. If we want to customize the output, we can 
override the ``do_listen()`` function in the room's code, like this:

.. code-block:: c

    void do_listen() { 
        write("You hear " + (random(3) + 2) + " animals fighting in the distance.");
    }

**Exercise 5**: 
   Add a new function called ``do_listen()`` to the room ``swamp1.c``. It should send the 
   message with fighting animals to the user. Remember, the function takes no parameters 
   and returns nothing.

Here's an example of the function:

.. code-block:: c

    void do_listen() { 
        write("You hear " + (random(3) + 2) + " animals fighting in the distance.");
    }

After updating the room, reload it and test the new functionality by repeatedly using the ``listen`` command.

.. tip::

    Use the following command to list all functions that begin with ``do_`` in the ``OUTDOOR_ROOM``:

.. code-block:: text

   ^pinto/room/> dbxfuncs /std/outdoor_room do_

    Matches:
    int do_verb_rule(x, x, x)     (defined in /std/object/vsupport)
    void do_search(x, x)          (defined in /std/object/vsupport)
    int do_not_restore()          (defined in /std/object)
    void do_receive(x, x)         (defined in /std/container)
    void do_go_str(x)             (defined in /std/modules/m_exit)
    void do_looking(x, x)         (defined in /std/room/roomdesc)
    void do_pray()                (defined in /std/base_room)
    **void do_listen()              (defined in /std/base_room)**
    void do_look_at_str()         (defined in /std/base_room)
    void do_smell()               (defined in /std/base_room)

Alternatively, use the ``apropos`` command to search for documented functions:

.. code-block:: text

    ^pinto/room/> apropos do_listen
    [autodoc/mudlib]:  do_listen
    ^pinto/room/>

**Exercise 6**:
   Use the ``::`` operator to call the original ``do_listen()`` function in addition to your 
   new functionality. This way, both the original behavior and your customized message will be triggered.

Here is the final code:

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
                          "get":"You stick your hands in the mud, look at them, "
                                "then decide there are better MUDs.",
                          "search":"You found some dirty hands."]));
    }

    void do_listen()
    {
        ::do_listen();
        write("You hear " + (random(3) + 2) + " animals fighting in the distance.");
    }


CHAPTER 3: Creating Weapons
============================

Creating weapons in the MUD follows a similar structure to creating rooms, especially with the 
use of the `setup()` function. This function is crucial for defining the weapon’s name, attributes, and 
other important details. Let's take a closer look at the code for the greataxe found in `^std/weapon/greataxe.c`:

.. code-block:: c
   :linenos:

   /* Do not remove the headers from this file! See /USAGE for more information. */

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
      set_salvageable((["wood":15, "metal":85]));
   }

Let’s break down this code line by line:

  - **Line 3**: We inherit the `WEAPON` object from `^std/weapon.c`. If this seems unfamiliar, 
    refer back to the LIMA Guide, Section 1.3, on shortcuts for filenames.
  - **Line 5**: The familiar `setup()` function initializes the weapon.
  - **Line 7**: We use `set_id()` to define the identifiers for the weapon. In this case, the 
    greataxe will respond to both "axe" and "greataxe". This allows players to type either `wield axe` or `wield greataxe`.
  - **Line 8**: This sets the weight of the weapon in kilograms.
  - **Line 9**: The value is set to 30 (currency details will be covered later when discussing the MONEY_D daemon).
  - **Line 10**: The weapon's class is set to 12, determining how much damage it can deal. 
    In this case, it can deal between 0 and 11 damage points, plus any strength-based bonuses. When dual-wielded, the weapon may apply 1.5 times the strength bonus.
  - **Line 11**: The combat messages are set here. These messages will be displayed during 
    combat actions. For now, you can explore the standard combat messages in the `/data/messages/` directory.
  - **Line 12**: This sets the type of damage the weapon inflicts, in this case, "slashing". 
    The various damage types are managed by the `DAMAGE_D` daemon.
  - **Line 13**: This line specifies which skill is trained by using the weapon. In this case, 
    it is the "combat/melee/blade" skill.
  - **Line 14**: A skill restriction is applied here, requiring the player to have at least 
    rank 1 in "combat/axe" to use the weapon effectively. Without this rank, the player can 
    still use the weapon but will experience reduced efficiency and damage output.

As you can see, the `setup()` function in weapons shares many similarities with how rooms 
are set up. It involves making various function calls to add features and behaviors to the object.

.. tip::

   Prefer to use a different unit for weight? You can switch from metric units by 
   changing `#define METRIC` to `#undef METRIC` in the `config.h` file.

.. note::

   The `DAMAGE_D` daemon also manages special weapon attacks. Think of something 
   like a "murderous longsword of lightning bolts," and you’ll get the idea.