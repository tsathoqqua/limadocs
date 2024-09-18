6 Creating NPCs
===============

**UNDER CONSTRUCTION**

6.2 Modules
-----------
The LIMA mudlib consists of more than 60 modules located in ``/std/modules``. These modules all got shorthand
names found in ``/include/mudlib.h`` like many other things. The modules *try* to be independent and modular,
but some still depend on others.

Ideally, the modules provides functionality that you can add to your code as needed. Need another feature?
Inherit another module. Here is the top most lines from ``^std/monster/robert.c``:

.. code-block:: c 

   inherit LIVING;       
   inherit M_NPCSCRIPT;
   inherit M_TRIGGERS;   
   inherit M_SMARTMOVE;

As you can see "Bob" inherits ``LIVING`` (a non combat monster), but also ``M_NPCSCRIPT`` that gives Bob
ability to use NPC scripts (more on those later). If you open the ``M_NPCSCRIPT`` file you will see:

.. code-block:: c 

   ...
   inherit M_ACTIONS;

So ``M_NPCSCRIPT`` requires ``M_ACTIONS``, a module that is requires for NPCs to take actions in the world (get, give, 
speak, etc.), but ``M_NPCSCRIPT`` can also do triggers, meaning wait for events in the MUD and act upon them. This
only works if ``M_TRIGGERS`` is also inherited. Finally, ``M_SMARTMOVE`` provides the ability for Bob to move.

All these abilities could be inherited always into ``M_NPCSCRIPT``, but this way gives you, as a programmer,
the option to extend the capabilities of Bob as you see fit, or reduce them to less. In general, we do not want
out MUD objects to inherit more things than what is needed - it is all about keeping the memory and CPU usage 
down to what is needed - the reverse is sometimes referred to as "bloat", "bloated code", or "bloated objects".

A short list of modules typically used for monsters is shown here:

  - :doc:`m_accountant <../module/modules-m_accountant>` - for NPCs running bank departments
  - :doc:`m_actions <../module/modules-m_actions>` - for NPCs that need to be able to take actions
  - :doc:`m_aggressive <../module/modules-m_aggressive>` - for NPCs that should attack on sight
  - :doc:`m_assistance <../module/modules-m_assistance>` - for NPCs that call for assistance
  - :doc:`m_blockexits <../module/modules-m_blockexits>` - for NPCs that should block exits
  - :doc:`m_boss <../module/modules-m_boss>` - for boss monsters
  - :doc:`m_companion <../module/modules-m_companion>` - for companions monsters that can be trained by the players
    and follow them around
  - :doc:`m_conversation <../module/modules-m_conversation>` - for interactive menus that allow players to talk to
    the NPCs in a RPG style chat, where one option can add or remove new options and give quests based on stats
    and skills.
  - :doc:`m_follow <../module/modules-m_follow>` - for monsters that should follow if the player leaves.
  - :doc:`m_npcscript <../module/modules-m_npcscript>` - for scripting sequences of steps for NPCs
  - :doc:`m_smartmove <../module/modules-m_smartmove>` - for monsters that should be able to move
  - :doc:`m_trainer <../module/modules-m_trainer>` - for NPCs that should be able to train skills or stats
  - :doc:`m_vendor <../module/modules-m_vendor>` - for NPCs that sell and buy stuff (shopkeepers, e.g.)
  - :doc:`m_wander <../module/modules-m_wander>` - for monsters that should wander around randomly

.. info::

    There is ongoing work with modules making them more independent. Earlier, they had hidden dependencies, e.g.
    module A depending on module B, but this has been changed to module A now inheriting module B so you do not
    have to figure out these dependencies. This does, however, mean that if you already inherit module B, and then
    start inheriting module A, you should no longer inherit module B in your code, as it is included in module A.


6.1 Banking NPCs
----------------

.. code-block:: c 
   :linenos:

    /* Do not remove the headers from this file! see /USAGE for more info. */

   inherit ADVERSARY;
   inherit M_ACCOUNTANT;

   void setup()
   {
      set_name("Samuel");
      add_id("accountant", "sam");
      set_gender(1);
      set_proper_name("Samuel the Bank Accountant");
      set_in_room_desc("Samuel the Bank Accountant stands behind the counter.");
      set_long("Samuel is a boring looking balding man. Perfectly clothed of "
               "course.");
      set_bank_id("Bean");
      set_bank_name("The Imperial Bank of the Bean");
      set_currency_type("gold");
      set_exchange_fee(5);
   }
