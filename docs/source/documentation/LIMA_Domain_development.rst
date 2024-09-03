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
