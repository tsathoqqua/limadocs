#########################
The Basics of Inheritance
#########################

You should now understand the basic workings of functions.  You should be
able to declare and call one.  In addition, you should be able to recognize
function definitions, although, if this is your first experience with LPC,
it is unlikely that you will as yet be able to define your own functions.
There functions form the basic building blocks of LPC objects.  Code
in them is executed when another function makes a call to them.  In making
a call, input is passed from the calling function into the execution of
the called one.  The called function then executes and returns a value
of a certain data type to the calling function.  Functions which return
no value are of type void.

After examining your workroom code, it might look something like this
(depending on the mudlib):

.. code-block:: c

   inherit INDOOR_ROOM; //See section 1.3 above.

   void setup()
   {
      set_brief("Cartesius's Workroom");
      set_long("It's pretty empty, seems like nobody is working here.");
      set_exits((["down":"^std/room/Wizroom"]));
   }

If you understand the entire textbook to this point, you should recognize
of the code the following:

    1. ``setup()`` is the definition of a function (hey! he did not declare it)
    2. It makes calls to ``set_brief()``, ``set_long()``, and ``set_exits()``, none
       of which are declared or defined in the code.
    3. There is a line at the top that is no variable or function declaration
       nor is it a function definition!

This chapter will seek to answer the questions that should be in your head
at this point:

    1. Why is there no declaration of ``setup()``?
    2. Where are the functions ``set_brief()``, ``set_long()``, and ``set_exits()`` declared
       and defined?
    3. What the hell is that line at the top of the file?

The Basics of Inheritance
=========================

Review
------

You should now understand the basic workings of functions.  You should be
able to declare and call one.  In addition, you should be able to recognize
function definitions, although, if this is your first experience with LPC,
it is unlikely that you will as yet be able to define your own functions.
There functions form the basic building blocks of LPC objects.  Code
in them is executed when another function makes a call to them.  In making
a call, input is passed from the calling function into the execution of
the called one.  The called function then executes and returns a value
of a certain data type to the calling function.  Functions which return
no value are of type void.

After examining your workroom code, it might look something like this
(depending on the mudlib):

.. code-block:: c

   inherit INDOOR_ROOM; //See section 1.3 above.

   void setup()
   {
      set_brief("Cartesius's Workroom");
      set_long("It's pretty empty, seems like nobody is working here.");
      set_exits((["down":"^std/room/Wizroom"]));
   }

If you understand the entire textbook to this point, you should recognize
of the code the following:

    1. ``setup()`` is the definition of a function (hey! he did not declare it)
    2. It makes calls to ``set_brief()``, ``set_long()``, and ``set_exits()``, none
       of which are declared or defined in the code.
    3. There is a line at the top that is no variable or function declaration
       nor is it a function definition!

This chapter will seek to answer the questions that should be in your head
at this point:

    1. Why is there no declaration of ``setup()``?
    2. Where are the functions ``set_brief()``, ``set_long()``, and ``set_exits()`` declared
       and defined?
    3. What the hell is that line at the top of the file?

Object oriented programming
===========================

*Inheritance* is one of the properties which define true object oriented
programming (OOP). It allows you to create generic code which can be used
in many different ways by many different programs.  What a mudlib does is
create these generalized files (objects) which you use to make very specific
objects (in *your mudlib*).

If you had to write the code necessary for you to define the workroom above,
you would have to write about 1000 lines of code to get all the functionality
of the room above - and for every workroom there after.  Clearly that is a 
waste of disk space. In addition, such code does not interact well with players and other rooms since every
creator is making up his or her own functions to perform the functionality
of a room.  Thus, what you might use to write out the room's long description,
``query_long()``, another wizard might be calling ``long()``.  This is the primary
reason mudlibs are not compatible, since they use different protocols and styles for
object interaction.

OOP overcomes these problems.  In the above workroom, you inherit the
functions already defined in a file called "/std/indoor_room.c".  It has all
the functions which are commonly needed by all rooms defined in it.  When
you get to make a specific room, you are taking the general functionality
of that room file and making a unique room by adding your own function,
``setup()``.

How inheritance works
=====================
As you might have guessed by now, the line:

.. code-block:: c

   inherit INDOOR_ROOM; 

has you inherit the functionality of the room "/std/indoor_room.c", a special
file for indoor rooms (Guess what ``OUTDOOR_ROOM`` does?). Remember from
section 1.3, how the shorthands for files like this are defined in ``/include/mudlib.h``,
so you can write ``inherit INDOOR_ROOM;`` instead of writing ``inherit "/std/indoor_room";``.
Why this is clever is described in section 1.3.

By inheriting the functionality, it means that you can use the functions which have
been declared and defined in the file ``/std/indoor_room.c``. 

In actual practice, each mudlib is different, and thus requires you to use
a different set of standard functions, often to do the same thing.  It is
therefore beyond the scope of this textbook even to describe what
functions exist and what they do.  LIMA, however, is documented 
on https://limamudlib.readthedocs.io/. Here you will be able to find documentation
for all the modules, functions, objects, commands and more needed to develop
your new mud with the mudlib.

Chapter summary
===============
This is far from a complete explanation of the complex subject of inheritance.
The idea here is for you to be able to understand how to use inheritance in
creating your objects. A full discussion will follow in a later guide.

Right now you should know the following:

    1. Each mudlib has a library of generic objects with their own general
       functions used by creators through inheritance to make coding objects
       easier and to make interaction between objects smoother.
    2. The functions in the inheritable files of a mudlib vary from mudlib
       to mudlib.  There should exist documentation on your MUD on how to
       use each inheritable file.  If you are unaware what functions are
       available, then there is simply no way for you to use them.  Always
       pay special attention to the data types of the input and the data
       types of ay output.
    3. You inherit the functionality of another object through the line:

.. code-block:: c

   inherit "filename";
       
where filename is the name of the file of the object to be inherited.
This line goes at the beginning of your code.

.. note::

   You may see the syntax ``::create()`` or ``::mudlib_setup()`` or even ``::reset()`` in places.
   You do not need fully to understand at this point the full nuances of this,
   but you should have a clue as to what it is. The "::" operator is a way
   to call a function specifically in an inherited object (called the scope
   resolution operator).  For instance, most MUDs' ``indoor_room.c`` or ``room.c`` has a function
   called ``create()``.  When you inherit ``room.c`` and configure it, you are doing
   what is called overriding the ``create()`` function in ``room.c``.  This means
   that whenever ANYTHING calls ``create()``, it will call *your* version and not
   the one in ``room.c``.  However, there may be important stuff in the ``room.c``
   version of ``create()``.  The ``::`` operator allows you to call the ``create()`` in
   ``room.c`` instead of your ``create()``.

An example:

.. code-block:: c

   //Example #1
   inherit "/std/room";

   void create() { create(); }

And another example:

.. code-block:: c

   // Example #2
   inherit "/std/room";

   void create() { ::create(); }

Example 1 is a horror.  When loaded, the driver calls  ``create()``, and then
 ``create()`` calls  ``create()``, which calls  ``create()``, which calls  ``create()``...
In other words, all  ``create()`` does is keep calling itself until the driver
detects a too deep recursion and exits.

Example 2 is basically just a waste of RAM, as it is no different from room.c
functionally.  With it, the driver calls its  ``create()``, which in turn calls
``::create()``, the ``create()`` function defined in ``room.c``.  
Otherwise it is functionally exactly the same as room.c.

Variable handling
=================

*Inheritance* is one of the properties which define true object oriented
programming (OOP). It allows you to create generic code which can be used
in many different ways by many different programs.  What a mudlib does is
create these generalized files (objects) which you use to make very specific
objects (in *your mudlib*).

If you had to write the code necessary for you to define the workroom above,
you would have to write about 1000 lines of code to get all the functionality
of the room above - and for every workroom there after.  Clearly that is a 
waste of disk space. In addition, such code does not interact well with players and other rooms since every
creator is making up his or her own functions to perform the functionality
of a room.  Thus, what you might use to write out the room's long description,
``query_long()``, another wizard might be calling ``long()``.  This is the primary
reason mudlibs are not compatible, since they use different protocols and styles for
object interaction.

OOP overcomes these problems.  In the above workroom, you inherit the
functions already defined in a file called "/std/indoor_room.c".  It has all
the functions which are commonly needed by all rooms defined in it.  When
you get to make a specific room, you are taking the general functionality
of that room file and making a unique room by adding your own function,
``setup()``.

How inheritance works
=====================
As you might have guessed by now, the line:

.. code-block:: c

   inherit INDOOR_ROOM; 

has you inherit the functionality of the room "/std/indoor_room.c", a special
file for indoor rooms (Guess what ``OUTDOOR_ROOM`` does?). Remember from
section 1.3, how the shorthands for files like this are defined in ``/include/mudlib.h``,
so you can write ``inherit INDOOR_ROOM;`` instead of writing ``inherit "/std/indoor_room";``.
Why this is clever is described in section 1.3.

By inheriting the functionality, it means that you can use the functions which have
been declared and defined in the file ``/std/indoor_room.c``. 

In actual practice, each mudlib is different, and thus requires you to use
a different set of standard functions, often to do the same thing.  It is
therefore beyond the scope of this textbook even to describe what
functions exist and what they do.  LIMA, however, is documented 
on https://limamudlib.readthedocs.io/. Here you will be able to find documentation
for all the modules, functions, objects, commands and more needed to develop
your new mud with the mudlib.

Chapter summary
===============
This is far from a complete explanation of the complex subject of inheritance.
The idea here is for you to be able to understand how to use inheritance in
creating your objects. A full discussion will follow in a later guide.

Right now you should know the following:

    1. Each mudlib has a library of generic objects with their own general
       functions used by creators through inheritance to make coding objects
       easier and to make interaction between objects smoother.
    2. The functions in the inheritable files of a mudlib vary from mudlib
       to mudlib.  There should exist documentation on your MUD on how to
       use each inheritable file.  If you are unaware what functions are
       available, then there is simply no way for you to use them.  Always
       pay special attention to the data types of the input and the data
       types of ay output.
    3. You inherit the functionality of another object through the line:

.. code-block:: c

   inherit "filename";
       
where filename is the name of the file of the object to be inherited.
This line goes at the beginning of your code.

.. note::

   You may see the syntax ``::create()`` or ``::mudlib_setup()`` or even ``::reset()`` in places.
   You do not need fully to understand at this point the full nuances of this,
   but you should have a clue as to what it is. The "::" operator is a way
   to call a function specifically in an inherited object (called the scope
   resolution operator).  For instance, most MUDs' ``indoor_room.c`` or ``room.c`` has a function
   called ``create()``.  When you inherit ``room.c`` and configure it, you are doing
   what is called overriding the ``create()`` function in ``room.c``.  This means
   that whenever ANYTHING calls ``create()``, it will call *your* version and not
   the one in ``room.c``.  However, there may be important stuff in the ``room.c``
   version of ``create()``.  The ``::`` operator allows you to call the ``create()`` in
   ``room.c`` instead of your ``create()``.

An example:

.. code-block:: c

   //Example #1
   inherit "/std/room";

   void create() { create(); }

And another example:

.. code-block:: c

   // Example #2
   inherit "/std/room";

   void create() { ::create(); }

Example 1 is a horror.  When loaded, the driver calls  ``create()``, and then
 ``create()`` calls  ``create()``, which calls  ``create()``, which calls  ``create()``...
In other words, all  ``create()`` does is keep calling itself until the driver
detects a too deep recursion and exits.

Example 2 is basically just a waste of RAM, as it is no different from room.c
functionally.  With it, the driver calls its  ``create()``, which in turn calls
``::create()``, the ``create()`` function defined in ``room.c``.  
Otherwise it is functionally exactly the same as room.c.


.. disqus::

