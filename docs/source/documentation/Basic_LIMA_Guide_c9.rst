CHAPTER 9: Verbs and interactions
=================================

9.1 Review
----------

You know understand objects, calling them, functions and variables. Now it is time to
look at how to create player interactions. 

Verbs should generally be used instead of commands for "in character" ("IC")
actions, ie actions which the character should have access to, rather than 
the player - eg "look" (IC) should be a verb, while "help" should not.
Lima does not support add_actions.

Reasons for this: Verbs have a central "condition checking" (is the character dead ? etc),
other checking for whether the action is possible is well-supported multiple syntaxes can 
easily be defined for each verb aliases can be defined for each syntax within each verb
sensible default error messages, easily tailored as required.

9.2 No add_action()
-------------------

Unlike many other mudlibs that you will meet LIMA does not use the efun ``add_action()``. 

LIMA uses verbs instead. Absolutely nothing remotely similar to ``add_action()`` 
exists in the lib. It is completely impossible for a room or object to add new commands
to the game.

The reason for this is consistency. With the LIMA mudlib, things work the same everywhere, 
making things much easier to understand.  Of course, many things may not do anything 
interesting; however they should at least give a reasonable error message. For example, if
there is anything in the MUD that can be twisted, it makes more sense for *everything* 
to be able to be twisted, and simply do nothing, instead of each object which can be 
twisted having to completely reimplementing the concept.

The ``add_action()`` way of doing things leads to lots of code duplication, and in many 
cases poor parsing since the person writing the command is more interested in getting 
it to work for him than doing any sort of general parsing; in many cases the person in
question is an area coder with little mudlib experience anyway.  As anyone who has worked 
extensively with ``add_action()`` knows, bringing any two such objects into close proximity 
often results in complete disasters, and rarely even succeeds in generating the correct error
message for most commands.

.. note::

    If you have *no idea* what ``add_action()`` is, you have nothing to unlearn here - which
    is good! Now you also know, that if an AI/someone on Reddit tries to produce LPC code for you, and it contains
    ``add_action()`` you should tell it/him/her off, as it is not creating LIMA compatible code.

9.3 How verbs work
------------------

9.4 Creating verbs
------------------

Verbs are defined in individual files within ``/cmds/verbs/``.

They inherit ``VERB_OB``.

The syntaxes (rules) and aliases allowed for the verb are defined using the 
``add_rules()`` function within ``create()``.

Each rule for the verb has a corresponding ``do_`` function.

Optional ``can_`` function for each syntax - if it doesn't evaluate to 1,
the action is prevented, using default error message if 0 is returned, 
whilst returning a string causes that to be used as the error message.

Verbs using any OBJ rule (ie. the rule involves an object) require a
``direct_`` function, similar to the optional ``can_`` function.

Verbs involving a second object require a similar ``indirect_`` function
for that second object.

Flags are used to signify which of the following checks are to be applied
in the verb:

  |  ``NEED_TO_SEE``
  |  ``NEED_TO_BE_ALIVE``
  |  ``NEED_TO_THINK``
  |  ``TRY_TO_ACQUIRE``

The first three are self-explanatory, and are included by default.

``TRY_TO_ACQUIRE`` is excluded by default, and adding it signifies that the verb
requires the object to be in the player's possession, and will try to acquire
it first.

Use ``add_flag()`` and ``clear_flag()`` to add/remove these condition checks,
and include verbs.h which is where they are defined.

Most objects will inherit ``/std/obj/vsupport.c``, which contains various 
default verb support functions (generally ``direct_``).

When evaluating ``direct_`` and ``indirect_`` functions, any such functions in 
either the verb or the object concerned will be checked, followed by the
generic ``direct_verb_rule()`` and ``indirect_verb_rule()`` functions.

A default version of ``direct_verb_rule()`` is included in ``/std/obj/vsupport.c``,
normally returning 1 for containers (rooms) and exits (provided the object 
passes the default checks, such as being visible).

Similarly a default version of ``indirect_verb_rule()`` was included in ``CONTAINER``
(``/std/container.c``), allowing moving things to/from containers by default
(put, get etc) - this has now been moved to ``/std/container/vsupport.c`` and
replaced by specific ``indirect_`` functions for the appropriate verbs.

Default implementations for various rules are included in ``VERB_OB``.
For example, the implementation of the OBJ rule calls ``do_verb()`` in the object,
after having made various checks.

9.5 Debugging verbs
-------------------

Use the `parse <../command/parse.html>`_ command (in front of the normal verb syntax) to see the 
results of can/direct/indirect checks, and hence which rule(if any) is
used.

9.6 Simple verb example
-----------------------

Let's try to invent a verb for kicking things.

.. code-block:: c
 
   inherit VEB_OB;

   void do_kick_obj(object ob)
   {
     ob->do_kick();
   }

   void create()
   {
     add_rules( ({ "OBJ" }) ({ }) );
   }

In any object which can successfully be kicked:

   1. Have a ``direct_kick_obj()`` function returning 1
   2. Have a do_kick() function which implements the effects of kicking it

eg a ball to kick:

.. code-block:: c
 
   inherit OBJ;

   void setup()
   {
     set_id("ball");
     set_long("It's a ball, sitting waiting to be kicked....");
   }

   mixed dirct_kick_obj() { return 1; }

   void do_kick()
   {
     this_body()->simple_action("$N $vkick $o.", this_object();
     // ADD SOME CODE TO MOVE IT TO A NEW ROOM
     // AND MESSAGE ON ENTERING THE ROOM
   }

It is usually worth abstracting such code into a module, so that similar
items can inherit the module, instead of cut/pasting the support code.

.. disqus::
