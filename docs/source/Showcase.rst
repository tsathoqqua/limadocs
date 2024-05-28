*************
LIMA Showcase
*************

============
Great layout
============

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
