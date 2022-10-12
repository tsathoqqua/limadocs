alias
=====

nomask void remove_alias(string alias_name)
-------------------------------------------

Remove an alias from an alias set.

void setup_for_save()
---------------------

Sets up M_SAVE to save some variables

add_alias(string name, string template, string* defaults, int xverb)
--------------------------------------------------------------------

Add an alias to an alias set.

mixed expand_alias(string input)
--------------------------------

Expand an argv with any aliases if applicable.