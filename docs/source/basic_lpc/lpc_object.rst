######################
The data type "object"
######################

You should now be able to do anything so long as you stick to calling
functions within your own object. You should also know, that at the
bare minimum you can get the ``setup()`` (or ``create()``) function in your object
called to start just by loading it into memory. Note that neither of these
functions MUST be in your object. The driver checks to see if the
function exists in your object first.  If it does not, then it does not
bother. You are also acquainted with the data types void, int, and string.
 
Objects as data types
=====================

In this chapter you will be acquainted with a more complex data type:
object.  An object variable points to a real object loaded into the
driver's memory. You declare it in the same manner as other data types:

.. code-block:: c
 
   object ob;

It differs in that you cannot use +, -, +=, -=, \*, or / (what would it
mean to divide a monster by another monster? I guess if it was the same monster it would be 1).
And since functions like ``tell()`` and ``write()`` only want strings or ints, you cannot 
``write()`` or ``tell()`` them (again, what would it mean to say a monster? Raawwwr?).

But you can use them with some other of the most important efuns on any LPMud.
 
The efun: this_object()
=======================

This is an efun which returns an object in which the function being executed
exists.  In other words, in a file, ``this_object()`` refers to the object your
file is in whether the file gets cloned itself or inherted by another file.

It is often useful when you are writing a file which is getting inherited
by another file.  Say you are writing your own living.c which gets
inherited by user.c and monster.c, but never used alone.  You want to log
the function ``set_level()`` it is a player's level being set (but you do not
care if it is a monster).

You might do this (LIMA already handles this function, but just an example):
 
.. code-block:: c
 
   void set_level(int x) 
   {
       if(this_object()->is_body()) log_file("levels", "foo\n");
       level = x;
   }
 
Since ``is_body()`` is not defined in ``adversary.c`` or anything it inherits,
just saying ``if(is_body())`` will result in an error since the driver
does not find that function in your file or anything it inherits.
``this_object()`` allows you to access functions which may or may not be
present in any final products because your file is inherited by others
without resulting in an error.

.. note::

    In FluffOS, a lot of places where an object is expected in an efun
    it will default use ``this_object()`` without having to write it.
    Experiment, and see if you can make your code shorter by leaving it out.

Calling functions in other objects
==================================

This of course introduces us to the most important characteristic of
the object data type.  It allows us to access functions in other objects.
In previous examples you have been able to find out about a player's level,
reduce the money they have, and how much hp they have.

Calls to functions in other objects may be done in two ways:
 
.. code-block:: c
 
   object->function(parameters)
   call_other(object, "function", parameters);
 
Example:

.. code-block:: c
 
   this_body()->add_money("gold", -5);
   call_other(this_body(), "add_money", "gold", -5);
 
In some (very loose sense), the game is just a chain reaction of function
calls initiated by player commands.  When a player initiates a chain of
function calls, that player is the object which is returned by
the sefun this_body().  So, since this_body() can change depending
on who initiated the sequence of events, you want to be very careful
as to where you place calls to functions in this_body().  

.. disqus::

