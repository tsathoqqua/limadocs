CHAPTER 2: The LPC Program
==========================

2.1 About programs
------------------

The title of this chapter of the textbook is actually poorly named, since
one does not write programs in LPC.  An LPC coder instead writes *objects*.
What is the difference?  Well, for our purposes now, the difference is
in the way the file is executed.  When you "run" a program, execution
begins at a definite place in the program.  In other words, there
is a place in all programs that is noted as the beginning where program
execution starts.  In addition, programs have definite end points,
so that when execution reaches that point, the execution of the program
terminates.  So, in short, execution of a program runs from a definite
beginning point through to a definite end point. This is not so with
LPC objects.

With muds, LPC objects are simply distinct parts of the C program which
is running the game (the *driver*).  In other words, execution of the mud
program begins and ends in the driver.  But the driver in fact does
very little in the way of creating the world you know when you play
a mud.  Instead, the driver relies heavily on the code created in LPC,
executing lines of the objects in the MUD as needed.  LPC objects thus
have no place that is necessarily the beginning point, nor do they
have a definite ending point.

Like other programming languages, an LPC "program" may be made up of
one or more files.  For an LPC object to get executed, it simple
needs to be loaded into the driver's memory.  The driver will call lines
from the object as it needs according to a structure which will be
defined throughout this textbook.  The important thing you need to
understand at this point is that there is no "beginning" to an LPC
object in terms of execution, and there is no "end".

2.2 Driver-mudlib interaction
-----------------------------

As mentioned earlier, the driver is the program that runs on
the host machine.  It connects you into the game and processes LPC code.
Note that this is one theory of MUD programming, and not necessarily
better than others.  It could be that the entire game is written in C.
Such a game would be faster, but it would be less flexible in that developes 
(sometimes called "wizards") could not add things to the game while it was running. 
This is the theory behind DikuMUDs. Instead, LPMUDs run on the theory that
the driver should in no way define the nature of the game, that the nature
of the game is to be decided by the individuals involved, and that
you should be able to add to the game *as it is being played*, e.g. being
able to rebuild and change the game while it is running.  This
is why LPMUDs make use of the LPC programming language.  It allows
you to define the nature of the game in LPC for the driver to read and
execute as needed. It is also a much simpler language to understand
than C, thus making the process of world creation open to a greater
number of simultaneous people.

Once you have written a file in LPC (assuming it is correct LPC), it justs
sits there on the hard drive of the host machine until something in the game
makes reference to it.  When something in the game finally does make
reference to the object, a copy of the file is (if it has no errrors) 
loaded into memory and a special *function* of that object is called 
in order to initialize the values of the variables in the object. 
Now, do not be concerned if that last sentence went right over your head, 
since someone brand new to programming would not know what the hell a function 
or a variable is. The important thing to understand right now is that a copy of the
object file is taken by the driver from the machine's hard drive and
stored into memory (since it is a copy, multiple versions of that
object may exist).  You will later understand what a function is, what
a variable is, and exactly how it is something in the game made reference
to your object.

2.3 Loading an object into memory
---------------------------------

Although there is no particular place in an object code that must exist in order for the driver 
to begin executing it, there is a place for which the driver will search in order to initialize 
the object. In classical mudlibs this is the function called ``create()``, but in LIMA the function
is called ``setup()``.

LPC objects are made up of variables (values which can change) and functions which are used to
manipulate those variables.  Functions manipulate variables through the use of LPC grammatical 
structures, which include calling other functions, using externally defined functions (often 
called "efuns"), and basic LPC expressions and flow control mechanisms.

Does that sound convoluted?  First lets start with a variable.  A variable might be something like: 
``level``. It can "vary" from sitation to situation in value, and different things use the value 
of the player's level to make different things happen.  For instance, if you are a level 19 player, 
the value of the variable level will be 19.  Basically, each object in LPC is a pile of variables 
with values which change over time. Things happen to these objects based on what values its variables
hold. Often, then things that happen cause the variables to change.

So, whenever an object in LPC is referenced by another object currently in memory, the driver searches
to see what places for values the object has (but they have no values yet).  Once that is done, the 
driver calls a function in the object called ``setup()`` which will set up the starting values for 
the object's variables.  It is thus through *calls* to *functions* that variable values get manipulated.

But ``setup()`` is NOT the starting place of LPC code, although it is where most LPC code execution 
does begin.  The fact is, those functions need not exist.  If your object does just fine with its
starting values all being default values, then you do not need a ``setup()`` function.  Thus
the first bit of execution of the object's code may begin somewhere completely different. LIMA uses
``create()`` internally since it uses the FluffOS driver, but as a developer using LIMA you would
rarely be confronted by a ``create()`` function, but most of the time use ``setup()``.

Now we get to what this chapter is all about.  The question: What consists a complete LPC object?  
Well, an LPC object is simply one or more functions grouped together manipulating zero or more
variables. The order in which functions are placed in an object relative to one another is 
irrelevant. In other words:

.. code-block:: c

   void setup() { set_name("gnat"); }
   void foo() { return; }
   int smile(string str) { return 0; }

is exactly the same as:

.. code-block:: c

   int smile(string str) { return 0; }
   void foo() { return; }
   void setup() { set_name("gnat"); }

Also important to note, the object containing only:

.. code-block:: c

   void nonsense() {}

is a valid, but trivial object, although it probably would not interact properly with other objects 
on your MUD since such an object has no weight, is invisible, etc.

2.4 Chapter summary
-------------------

LPC code has no beginning point or ending point, since LPC code is used to create objects to be used 
by the driver program rather than create individual programs.  LPC objects consist of one or more 
functions whose order in the code is irrelevant, as well as of zero or more variables whose
values are manipulated inside those functions.  LPC objects simply sit on the host machine's hard 
drive until referenced by another object in the game (in other words, they do not really exist). 
Once the object is referenced, it is loaded into the machine's memory with empty values for the variables. 
The function ``setup()`` (but really ``create()``) is called in that object if it exists to allow
the variables to take on initial values.  Other functions in the object are used by the driver and 
other objects in the game to allow interaction among objects and the manipulation of the LPC variables.

.. note::

   ``create()`` is called in the driver, but LIMA picks it up and does a lot of basic initialisations
   for your objects, which is why you should use ``setup()`` instead for normal objects that exist
   in the game world, i.e. torches, swords, trolls and laser pistols. For other objects that are not
   directly cloned into existance, like daemons, they still use create() to initialize when instantiated.

   Think of it like this: If your player is likely to interact with it (give, get, drop, look at) in the
   game world, it likely uses ``setup()``, if it's an object handling docking of spaceships, i.e. a game
   controlling object, it likely uses ``create()``.

   LIMA also handles resetting rooms automatically, this is done using the ``reset()`` function, but
   you do not need to know details on that right now.

.. disqus::
