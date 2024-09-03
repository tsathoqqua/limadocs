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

    1. Open 'admtool'
    2. Goto the privileged menu '1'
    3. Goto domains edit
    4. Create the pinto domain, 'c pinto'.
    5. Verify the domain creating succeeded by going to ``/domains`` and list the files there.

A number of directories have been created inside ``/domains/pinto/`` which will be used during
this guide.

  * Armour: Armours, hats, gloves, boots. Things that can be worn with or without protective effect.
  * Consumable: Food, drinks, drugs, healing items, poisons, etc.
  * Item: Quest items, items for players that can be picked up and typically saved with the player's
  * Mob: Monsters that can be fought and/or move around the area (hence mobiles).
  * Npc: Quest givers, shop keepers and other people/monsters that are typically not fought.
  * Obj: Objects for rooms, furniture, quest items that cannot be picked up by players.
    inventory.
  * Room: Areas are saved in series of connected rooms.
  * Weapon: Sword, grenades, clubs, pistols, rocks, anything that can be used as a weapon.

.. note::

   An admin is simply a wizard who belongs to the admin domain. You can grant admin status to a wizard by 
   adding them to this domain using the admtool. Domain controls are located under option '1' (admin 
   privilege), then "Domain".

.. warning::

    Do not delete domains that are already in LIMA when you first install it. Some of them are part of
    the security system, and deleting them will render LIMA defunct.
