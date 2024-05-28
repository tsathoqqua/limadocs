*************
LIMA Showcase
*************

============
Great layout
============

LIMA supports fancy frames player commands, and include both horizontal and vertical headers. The frames
can be themed in different colours and styles using the `frames command <../player_command/frames.html>`_, 
either using UTF-8 characters or simple ASCII characters. If your MUD client does not support UTF-8, 
you will automatically receive the ASCII version. The frames will automatically fit the width of your 
screen or whatever you select using the `width <../player_command/width.html>`_ player command. 

.. figure:: images/frames1.png
  :width: 700
  :alt: skills command

  Example of the `skills <../player_command/skills.html>`_ command.

Depending on the command, some commands may adopt their internal layout depending on the 
width of your screen. The frames also support using auto-scaling widgets for illustrating progress, 
balance between good and bad and more.

.. figure:: images/frames2.png
  :width: 700
  :alt: score command

  Example of the `score <../player_command/score.html>`_ command.

---------------------
Developer information
---------------------

Each frame defines accent and warning colours that can be used as standard when doing layout.

Useful modules to read:
- `Module: m_frame <module/modules-m_frame.html>`_
- `Module: m_widgets <module/modules-m_widgets.html>`_
