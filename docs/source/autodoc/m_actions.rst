m_actions
=========

void set_delay_time(int x)
--------------------------

 beneficial.

void do_game_command(string str)
--------------------------------

module.  E.g. do_game_command("wield sword").  do_game_command("smile hap*").

void respond(string str)
------------------------

me BEFORE the message gets to you.

protected void set_actions(int delay, string *actions)
------------------------------------------------------

 This function should only be called from within setup().