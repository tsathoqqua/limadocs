Module *m_conversation*
************************

Documentation for the modules-m_conversation module in */std/modules/m_conversation.c*.

Module Information
==================

M_CONVERSATION for doing interactive conversations with NPCs.

Three things are needed to run conversations::
  - A set of conversations options - set_options()
  - A set of matching replies for the options - set_responses()
  - A set of options to start from - set_start()

Both the replies and the options have special syntaxes that is used to control
the interactions. See the functions below for syntax descriptions.

This module supports filtering out certain options depending on the *stats* or *skills* of
the player. These requirements are specified in the options the players can chose and will
be limited accordingly. The filters are just added to the end of the reply option string,
examples follow below.

Special option syntax:
  - [str>10] - only shown if strength larger than 10
  - [str=10] - only shown if strength equals 10
  - [str<10] - only shown if strength less than 10.

Stats are: str, agi, int, con, cha.

Example:
    ``"I can probably lift that tree trunk for you?[str>60]"``

This also works for skills for <, > and =:

Example:
    ``"I can shoot that pistol right out of your hand [combat/ranged/pistol>3]"``

The numbers for skills refer to the ranks from 1-20. (See SKILL_D documentation)

For responses (from the NPC), some extra syntax allows you to open up new options and/or
close others. The responses can also be different types

Special option syntax:

   ``@@add options@@remove options``

The options can be seperated by "," to include more options.

Example:
    ``"I'll say something.@@add_option1,add_option2@@remove_option3"``

A response can be 4 different things:

  - A simple string that will be said by the NPC - "Hi there!"
  - An emote that must be performed - "!wave"
  - A function pointer - (: check_quest item :)
  - An array combined of all three things above.

Example:
    ``set_responses("hello":({"Hi there!","!wave",(: check_quest_item:) }));``

.. TAGS: RST

Functions
=========
.. c:function:: void set_goodbye(mixed arg)

This action is used when the NPC says goodbye.


.. c:function:: void add_option(string key, mixed act)

Adds a single option to the actions available.
See also set_option() documentation.


.. c:function:: void remove_option(string key)

Removes a single option from the actions available.
See also add_option() documentation.


.. c:function:: void add_options(mapping m)

Adds a mapping of options to the actions available.
See also set_option() documentation.


.. c:function:: void add_response(string key, mixed act)

Adds a single key and response to the responses available.


.. c:function:: void add_responses(mapping m)

Adds a mapping of responses to be available.


.. c:function:: void add_to_start(string key)

Adds a start option for everyone.


.. c:function:: varargs void add_start(mixed *a, object target)

Adds a start option for a specific target.


.. c:function:: void set_can_talk(int i)

Can be used to turn off if the NPC can talk or not, e.g. if they are
moving to a different location they might not be able to talk while moving.


.. c:function:: void set_options(mapping m)

Set a mapping of keys and options. These options are typically things the player says in the conversation and can
select from. Only keys added using set_start() will be shown initially. Other options can be introduced later in the
conversation using the add and remove syntax described in the set_responses() function.


.. c:function:: void set_responses(mapping m)

Set a mapping of keys (that must match the option keys), and responses. The responses use a special syntax described
below, that will allow adding and removing new options.


.. c:function:: varargs void set_start(mixed *a, object target)

Sets the options that the menu contains initially.


.. c:function:: void show_menu(object ob)

Shows the conversation menu to ob.


.. c:function:: void do_action(object ob, mixed action)

Do a specific action whether it's talking, calling a function, training or doing an emote.


.. c:function:: void bye(object ob)

Handle goodbye for ob.


.. c:function:: void exit_conversations()

Exit the conversation if the NPC needs to leave.
Default is to say "Sorry, I have to go", but this can
be changed using ``set_goodbye(action)``.


.. c:function:: void continue_conversation(object ob, string input)

Continue the conversation with ob given specific input.
Used internally in the menu system.


.. c:function:: string *filter_start(string *a, object body)

Override this function, to filter start options for a specific body.
See M_GUILD_MASTER for an example where the guild master adds an option to
join or leave the guild depending on the state of the body.


.. c:function:: void begin_conversation()

Begins the conversation for this_body(). The start options are default start options,
but filtered through the filter_start() function.



*File generated by Lima 1.1a4 reStructured Text daemon.*
