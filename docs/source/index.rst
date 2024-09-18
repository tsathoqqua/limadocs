LIMA Mudlib Documentation
=========================

**LIMA** (ˈlaɪmə) is a FluffOS based Mudlib originally developed in the 1990's.

For features of the LIMA Mudlib, please see our :doc:`showcase <Showcase>` which has lots of examples
of the features supported.

Check out the :doc:`usage <Usage>` page for further information. A small helper on :doc:`messaging <documentation/Messaging>` 
and :doc:`how to use the AUTODOC system <documentation/Autodocs>` inside the mudlib is also available.
:doc:`Skills in LIMA <documentation/Skills_LIMA>` describes the skill system, and how to 
configure it for your purpose.

Installation
------------
We recommend installation on Ubuntu, Debian or in WSL in Windows, see how to :doc:`install <Installation>`.

Latest changes
--------------
The latest changes to LIMA, can be read for version :doc:`1.1 alpha 4  <versions/11a4>`, latest release notes 
for :doc:`1.1 alpha 3 <versions/11a3>`. All known changes at this point can be read on the
:doc:`Versions page <Versions>`.

Learning paths
--------------
We provide several learning paths here with exercises to facilitate better leaning:

1. :doc:`LPC Basics learning path <basic_lpc>` is for people
   new to programming, LPC and MUDs.

2. :doc:`LIMA Domain Development <domain_dev>` will teach you about 
   developing your own areas, rooms, monsters, shopkeepers, weapons, armour, items, and a 
   small quest.

Commands
--------
- :doc:`Player Commands <Player_Commands>` - commands for players
- :doc:`Verbs <Verbs>` - common interactions with the mudlib
- :doc:`Other commands <Commands>` - mostly for wizards and admins

Mudlib
------
- :doc:`Daemons <Daemons>` - documentation and functions
- :doc:`Modules <Modules>` - modules
- :doc:`Mudlib <Mudlib>` - mudlib stuff
- :doc:`In game help <Ingame>` - in game help files
- :doc:`API <API>` - and other things

.. note::

   The LIMA documentation project is under active development.

.. toctree::
   :caption: LIMA
   :maxdepth: 3
   :hidden:
   :includehidden:

   Installation
   Versions
      
.. toctree::
   :caption: Learning
   :hidden:
   :includehidden:
   :maxdepth: 2

   basic_lpc
   domain_dev

.. toctree::
   :caption: Automated Docs
   :hidden:
   :includehidden:
   :maxdepth: 4

   documentation/Autodocs
   Player_Commands
   Verbs
   Commands
   Daemons
   Modules
   Mudlib
   Ingame
   API
