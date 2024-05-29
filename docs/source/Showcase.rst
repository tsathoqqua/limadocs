*************
LIMA Showcase
*************

This page shows a few of the newer features of the LIMA mudlib. These are all on top of:

- Centralized natural language parsing (zork like commands for players).
- Socials use the natural language parsing, and are easy to extend. (The soul is *huge*).
- Wizards have featureful shells including full unix globbing (ls \*.[ch]), command piping / redirection, etc...
- Inline documentation.	
- Distributed support.
- Very modular, with clear code.
- Support for Intermud-3.
- Modal input, and fully featured interactive shells.
- Real, secure security authored by Ellery @ TMI-2 (Reimer Behrends).
- Emphasis on player usability: better news, channels, menus, etc...	than any other mudlib
- Easy to build menus, easy to write socials, etc...
- A menu driven admin tool to ease routine administration tasks.

=====================
Dynamic modern layout
=====================

LIMA supports fancy frames player commands, and include both horizontal and vertical headers. The frames
can be themed in different colours and styles using the `frames command <../player_command/frames.html>`_, 
either using UTF-8 characters or simple ASCII characters.

.. figure:: images/frames1.png
  :width: 700
  :alt: skills command

  Example of the `skills <../player_command/skills.html>`_ command.

The frames will automatically fit the width of your  screen or whatever you select 
using the `width <../player_command/width.html>`_ player command. 

.. figure:: images/frames3.png
  :width: 700
  :alt: skills command in a smaller format

  Example of the `skills <../player_command/skills.html>`_ command on a narrow screen.

Depending on the command, some commands may adopt their internal layout depending on the 
width of your screen. The frames also support using auto-scaling widgets for illustrating progress, 
balance between good and bad and more.

.. figure:: images/frames2.png
  :width: 700
  :alt: score command

  Example of the `score <../player_command/score.html>`_ command.

If your MUD client does not support UTF-8, you will automatically receive the ASCII version. Several 
style options can be selected, like 'single', 'lines', 'ascii', 'double', but also 'none' to remove
all frames, e.g. if you are using a screen reader they might not be pleasant on your ears.

.. figure:: images/frames4.png
  :width: 700
  :alt: skills command in a ASCII mode

  Example of the `skills <../player_command/skills.html>`_ command in ASCII mode.


---------------------
Developer information
---------------------

Each frame defines accent and warning colours that can be used as standard when doing layout. The
frame library will do a lot of the work for you, but you have to check yourself when the width is
so wide/slim that the layout needs to change.

Example of code:

.. code-block:: c

   set_frame_title("Mail Groups");
   set_frame_left_header();
   set_frame_header(header);
   set_frame_content(output);
   out(frame_render());

Useful module documentation to read:

- `Module: m_frame <module/modules-m_frame.html>`_
- `Module: m_widgets <module/modules-m_widgets.html>`_

=======================
Marked up documentation
=======================

The auto documentation system in LIMA (which was orignally inspired by JAVA), collects mark-up from source
files, and creates help pages and fills the help system with topics. Two types of files exist:

Markdown, or .md are files typically copied from the driver and are mostly for Wizards.

.. figure:: images/documentation1.png
  :width: 700
  :alt: reStructured text view

And reStructured Text, or .rst files are documentation for player commands, wizard commands and more. They 
provide coloured mark up on the MUD, as well as online (here).

.. figure:: images/documentation2.png
  :width: 700
  :alt: Markdown view

The wizard `apropos command <command/apropos.html>`_ will provide a list of help pages and functions that
match the query, and a following 'man add_start', e.g., will then bring up the entire help page for 
`module m_conversation <module/modules-m_conversation.html>`_.

---------------------
Developer information
---------------------

For more on what you need to do, to continue to use the auto documenation system, see 
the page on the `AUTODOC system <documentation/Autodocs.html>`_.

=========================
Menu driven configuration
=========================

A lot of options in the mudlib can be configured via config files under */include/config/*. All these files
have a special format that includes instructions for range of values, default values and more.

.. code-block:: c

  // Each skill will start at 0, and go to this number of points.
  // Default: 10000
  // Range: 5000-30000
  // Type: integer
  #define MAX_SKILL_VALUE 10000

All the config files can modify the mudlib in fundamental ways, and can be configure using the 
`admtool command <command/admtool.html>`_.

.. figure:: images/menu_config1.png
  :width: 700
  :alt: Config file editing via admtool
