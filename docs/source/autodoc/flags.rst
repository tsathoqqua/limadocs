flags
=====

nomask int get_flags(int set_key)
---------------------------------

Any 'get' function for the flag set is also used.

private void set_flags(int which, int state)
--------------------------------------------

called.

varargs nomask void configure_set(
  int set_key,
  int is_non_persistent,
  function change_func
)
---------------------------------------------------------------------------------------

and a function that can be called when a flag changes.

nomask int test_flag(int which)
-------------------------------

includes information both about which flag set and which bit.

nomask void set_flag(int which)
-------------------------------

includes information both about which flag set and which bit.

nomask void clear_flag(int which)
---------------------------------

includes information both about which flag set and which bit.

nomask void assign_flag(int which, int state)
---------------------------------------------

both about which flag set and which bit.