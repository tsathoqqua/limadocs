.. index:: pair: LIMA Domain Develpoment; LIMA Mudlib
   :name: domain-dev

#######################
LIMA Domain Development
#######################

Prerequisites
=============

To fully benefit from this guide, you should have a basic understanding of LPC and how 
LPMUDs function. This foundational knowledge can be acquired by following the 
:doc:`LPC Basics learning path <basic_lpc>`. By this point, you should be familiar with variables, 
function declarations, function calls, objects, control flows, loops, some basic Linux commands 
that are helpful for working with the MUD, and the file structure used.

Help Improve This Document
--------------------------
This guide is designed for the latest features of the FluffOS driver and LIMA Mudlib.

If you notice any errors or omissions, please submit an issue at https://github.com/tsathoqqua/limadocs/. 
Describe the problem you found or suggest an addition. Even better, feel free to submit a pull request 
with your proposed changes.

Introduction
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


.. disqus::

.. toctree::
   :caption: LIMA Domain Development
   :numbered:
   :maxdepth: 4
   :titlesonly:

   domain_dev/dom_create

