m_save
======

protected void add_save(mixed *vars)
------------------------------------

Mark a variable as one that gets saved.

string *get_saved()
-------------------

returns the array of variables that get saved.

protected void set_save_recurse(int flag)
-----------------------------------------

sets whether or not a save is recursive.

varargs string save_to_string(int recursep)
-------------------------------------------

saves an object into a string representation.