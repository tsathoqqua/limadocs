##############
Creating rooms
##############
Time to create our first room! The first things to decide is which type of room you want to create:

   1. An outdoor room - OUTDOOR_ROOM
   2. An indoor room - INDOOR_ROOM
   3. A room filled with water - WATER_ROOM


A simple room
=============
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

Adding smell and sound
======================
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

Adding details
==============
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


Verbs and rooms
===============

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



