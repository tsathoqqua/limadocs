****************
Basic LIMA Guide
****************

Introduction
============

This guide is a shortened form of the original guide, with updated links to modern resources used in 2024. 
Some of the links in this guide may be outdated depending on at which time you are reading this, so use your
favourite search engine to find the location of the new URL or a similar tool that works in 2080 or whenever
you read this.

LPC is a very easy programming language to learn, and it has real value in that place most of us know as 
the real world. Much of the syntax used for LPC, can be used for other programming languages.

You need knowledge of basic Linux (or Linux) commands like: ls, cd, mkdir, mv, rm, etc.

* A nice course by Ubuntu (51 minutes): https://ubuntu.com/tutorials/command-line-for-beginners 

* A command reference: https://mally.stanford.edu/~sr/computing/basic-Linux.html

For the purpose of this guide, we will assume you will be using Visual Studio Code (or an editor with similar
functionality). Code can be downloaded at https://code.visualstudio.com/download.

If you are editing locally, you can use Code directly to edit your files, otherwise the editor also
provides the ability to edit remote files. Using an SFTP extension, e.g., can provide a way of editing remote 
files transparently, or just using the "Open a Remote window" functionality inside Code. Find the best way 
that suits your setup.

So, at this point, it is assumed you know how to edit and write a file, and some basic Linux commands. 

If you know C, you are handicapped in that LPC looks a lot like C, but it is not C.  Your preconceptions about
modular programming development will be a hinderence you will have to overcome.  If you have never heard of the 
C programming language, then you are only missing an understanding of the simple constructs of C like the flow 
of program execution and logical operators and such.  So a C guru has no real advantage over you, since
what they know from C which is applicable to LPC is easy to pick up. The stuff they know about C which makes 
them a guru is irrelevant to LPC.
 
The chapters of this manual are meant to be read in order.  Starting with the introduction, going sequentially 
through the chapter numbers as ordered in the contents file.  Each chapter begins with a paragraph or two 
explaining what you should have come to understand by that point in your studies.  After those introductory 
paragraphs, the chapter then begins to discuss its subject matter in nauseating detail.  At the end of the 
chapter is a briefly worded summary of what you should understand from that chapter if it has been successful. 
Following that may or may not be some sidenotes relevant to the subject at hand, but not necessary to its 
understanding.
 
If at any time you get to a chapter intro, and you have read the preceeding chapters thoroughly and you do not 
understand what it says you should understand by that point, please contact us on the LIMA MUD.
 
Some basic terms this manual uses:

* **Driver** This is the executable program which is the game (typically FluffOS, DGD or LDMUD - LIMA uses 
  FluffOS).  It accepts incoming connections, interprets LPC code defined by the mudlib, keeps MUD objects 
  in memory, makes periodic attempts to clean unused MUD objects from memory, makes periodic calls to objects, 
  and so on. This is the Unreal Engine of MUDs.
 
* **Mudlib** LIMA is a mudlib, that contains LPC code which defines the world in which you are in.  The driver of 
  itself is not a game. It is just a program which allows the creation of a multi-user environment.  
  In some sense, the driver is like an LPC compiler, and the mudlib is like a compiler's library 
  (a very loose analogy).  The mudlib defines basic objects which will likely be used over and over again by 
  people creating in the MUD world.  Examples of such objects are ``/std/base_room``, ``/std/body.c``, and so on.

* **Your mudlib** Your mudlib begins as a copy of a mudlib, in this case LIMA. From this point on, you
  can modify it as you want, add and subtract as you see fit. Modifying the files contained in the LIMA 
  library might break the library and introduce changes that are not compatible with the future LIMA development,
  but there are ways to stay compatible with LIMA and still introduce changes, more in this later.

* **Domain** Your game files live in domains folders under ``/domains/``. Each domain has a lead developer, and
  perhaps other developers to help develop that domain. You can think of a domain as an area or specific scope of 
  your MUD, like a forest area, a space station or a dungeon area. Anything that seems self-contained and should
  be worked on by a subset of the developers.

* **Object** A room, a weapon, a monster, a player, a bag, etc.  More importantly, every individual file with 
  a .c extension is an object.  Objects are used in different ways.  Objects like ``/std/living.c`` are 
  inherited by objects like ``/std/body.c`` (used for all players) and ``/std/adversary.c`` 
  (used for all things that can be fought). Others are cloned, which means a duplicate of that code is loaded 
  into memory, e.g. all pistols or longswords rely on the same code, but are independent objects in the game
  world. And still others are simply loaded into memory to be referenced by other objects, a typical example
  of these are called daemons, more on those later.

.. disqus::


2 The LPC Program
=================

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

In LPC programming, a variable is like a labeled box where you store a piece of information, like a number or a word. 
You can change what’s inside the box whenever you need to. For example, one box might hold your score in a game, 
and another might hold your name. These boxes (variables) help the program remember things and use them later!

An example of a variable:

.. code-block:: c

   int number_of_goblins = 42;

``int`` is the type of the variable and this must match ``42``, ``number_of_goblins`` is the *name* of the variable - the name
of the labeled box where we store our information. We give them names so they are easier to refer to, as referring to a position
in the memory of the computer is not easy at all.

Here is another example which introduces global and local variables:

.. code-block:: c

   int count;           // Global variable
   string name;         // Global variable

   void my_function() 
   {
       int local_var = 5;  // Local variable
   }

More on these later. The important thing to understand about variables right now is that a copy of the
object file is taken by the driver from the machine's hard drive and
stored into memory (since it is a copy, multiple versions of that
object may exist).  You will later fully understand what a function is, what
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


3 LPC Data Types
================

3.1 What you should know by now
-------------------------------

LPC object are made up of zero or more variables manipulated by one or more functions.  The order in 
which these functions appear in code is irrelevant.  The driver uses the LPC code you write 
by loading copies of it into memory whenever it is first referenced and additional copies
through cloning.  When each object is loaded into memory, all the variables initially point to no value. 
The ``setup()`` function in LIMA is used to give initial values to variables in objects.  The function 
for creation is called immediately after the object is loaded into memory. However, if you are reading 
this page with no prior programming experience, you may not know what a function is or how it gets 
called.  And even if you have programming experience, you may be wondering how the process of 
functions calling each other gets started in newly created objects.  Before any of these questions 
get answered, however, you need to know more about what it is the functions are
manipulating.  You therefore should thouroughly come to know the concept behind LPC data types.
Certainly the most boring subject in this manual, yet it is the most crucial, as 90% of all 
errors (excepting misplaced ``{}`` and ``()``) involve the improper usage of LPC data types.  
So bear through this important chapter, because it is my feeling that understanding this
chapter alone can help you find coding much, much easier.

3.2 Communicating with the computer
-----------------------------------

You possibly already know that computers cannot understand the letters and numbers used by humans.
Instead, the "language" spoken by computers consists of an "alphabet" of 0's and 1's.  
Certainly you know computers do not understand natural human languages.  But in fact, they do not
understand the computer languages we write for them either.  Computer languages like BASIC, C, 
C++, C#, etc. are all intermediate languages.  They allow you to structure your thoughts 
more coherently for translation into the 0's and 1's of the computer's languages.

There are two methods in which translation is done: compilation and interpretation.  These simply
are differences betweem when the programming language is translated into computer language.  With
compiled languages, the programmer writes the code then uses a program called a compiler to 
translate the program into the computer's language.  This translation occurs before the program
is run.  With interpreted languages however, the process of translation occurs as the program is 
being run.  Since the translation of the program is occurring during the time of the program's 
running in interpreted languages, interpreted languages make much slower programs than
compiled languages.

The bottom line is, no matter what language you are writing in, at some point this has to be 
changed into 0's and 1's which can be understood by the computer.  But the variables which you store in
memory are not simply 0's and 1's.  So you have to have a way in your programming languages of 
telling the computer whether or not the 0's and 1's should be treated as decimal numbers or characters or
strings or anything else.  You do this through the use of data types.

For example, say you have a variable which you call "x" and you give
it the decimal whole number value 65.  In LPC you would do this through
the statement:

.. code-block:: c

   x = 65;

You can later do things like:

.. code-block:: c

   write(x+"\n");        /* \n is symbolically represents a carriage return */
   y = x + 5;

The first line allows you to send 65 and a carriage return to someone's screen. The second line 
lets you set the value of y to 70. The problem for the computer is that it does not know what '65' 
means when you tell it ``x = 65;``.  What you think of 65, it might think of as:

        ``00000000000000000000000001000001``

But, also, to the computer, the letter 'A' is represented as:

        ``00000000000000000000000001000001``

So, whenever you instruct the computer to ``write(x+"\n");``, it must have some
way of knowing that you want to see '65' and not 'A'.

The computer can tell the difference between '65' and 'A' through the use of data types.  A data 
types simply says what type of data is being stored by the memory location pointed to by a 
given variable.  Thus, each LPC variable has a variable type which guides conversions. In the example
given above, you would have had the following line somewhere in the code *before* the lines shown above:

.. code-block:: c

  int x;

This one line tells the driver that whatever value ``x`` points to, it will be used as the data type 
"int", which is short for integer, or whole number. So you have a basic introduction into the reason 
why data types exist. They exist so the driver can make sense of the 0's and 1's that the computer 
is storing in memory.

3.3 The data types of LPC
-------------------------

All LPMud drivers have the following data types:

.. code-block:: c

    void, status, int, string, object, int *, string *, object *, mixed *

Many drivers, but not all have the following important data types which
are important to discuss:

.. code-block:: c

    class, float, mapping, float *, mapping *

And there are a few drivers with the following rarely used data types
which are not important to discuss:

.. code-block:: c

    function, enum, struct, char

3.4 Simple data types
---------------------

This introductory page will deal with the data types void, status, int, float, string, object, and 
mixed. This chapter deals with the two simplest data types (from the point of view of the LPC 
coder), int and string.

An int is any whole number.  Thus 1, 42, -17, 0, -10000023 are all type int. A string is one or 
more alphanumeric characters.  Thus "a", "We are Borg", "42", "This is not a string" are all strings.
Note that strings are always enclosed in "" to allow the driver to distinguish between the int 42 and
the string "42" as well as to distinguish between variable names (like ``x``) and strings by the same 
names (like "x").

When you use a variable in code, you must first let the driver know what type of data to which that 
variable points.  This process is called *declaration*.  You do this at the beginning of the function
or at the beginning of the object code (outside of functions before all functions which use it). 
This is done by placing the name of the data type before the name of the variable like in the following example:

.. code-block:: c

   void add_two_and_two()
   {
       int x;
       int y;

       x = 2;
       y = x + x;
   }

Now, this is a complete function.  The name of the function is ``add_two_and_two()``.  The function 
begins with the declaration of an int variable named ``x`` followed by the declaration of an 
in variable named ``y``.  So now, at this point, the driver now has two variables which
point to NULL values (meaning 0 typically), and it expects what ever values end up there 
to be of type int.

.. note::

   Void is a trivial data type which points to nothing.  It is not used
   with respect to variables, but instead with respect to functions.  You
   will come to understand this better later.  For now, you need only
   understand that it points to no value.  

   The data type status is a boolean data type.  That is, it can only have
   1 or 0 as a value.  This is often referred to as being true or false.

3.5 Chapter summary
-------------------

For variables, the driver needs to know how the 0's and 1's the computer
stores in memory get converted into the forms in which you intend them
to be used.  The simplest LPC data types are void, status, int, and string.
You do not user variables of type void, but the data type does come
into play with respect to functions.  In addition to being used for
translation from one form to the next, data types are used in determining
what rules the driver uses for such operations as +, -, etc.  For example,
in the expression 5+5, the driver knows to add the values of 5 and 5
together to make 10.  With strings however, the rules for int addition
make no sense.  So instead, with "a"+"b", it appends "b" to the string "a"
so that the final string is "ab".  Errors can thus result if you mistakenly
try to add "5"+5.  Since int addition makes no sense with strings, the
driver will convert the second 5 to "5" and use string addition.  The final
result would be "55".  If you were looking for 10, you would therefore
have ended up with erroneous code.  Keep in mind, however, that in most
instances, the driver will not do something so useful as coming up with
"55".  It comes up with "55" cause it has a rule for adding a string
to an int, namely to treat the int as a string.  In most cases, if you
use a data type for which an operation or function is not defined
(like if you tried to divide "this is" by "nonsense", "this is"/"nonsense"),
the driver will barf and report an error to you.

.. disqus::


4 Functions
===========
This chapter will teach you about functions, how to define them, how they work,
and how to call them.

4.1 Review
----------

By this point, you should be aware that LPC objects consist of functions
which manipulate variables.  The functions manipulate variables when they
are executed, and they get executed through *calls* to those functions.
The order in which the functions are placed in a file does not matter.
Inside a function, the variables get manipulated.  They are stored in
computer memory and used by the computer as 0's and 1's which
get translated to and from useable output and input through a device
called data typing.  String data types tell the driver that the
data should appear to you and come from you in the form of alphanumeric
characters.  Variables of type int are represented to you as whole
number values.  Type status is represented to you as either 1 or 0.
And finally type void has no value to you or the machine, and is not
really used with variable data types.

4.2 What is a function?
-----------------------

Like math functions, LPC functions take input and return output.
Languages like Pascal distinguish between the concept of proceedure abd
the concept of function.  LPC does not, however, it is useful to
understand this distinction.  What Pascal calls a proceedure, LPC
calls a function of type void.  In other words, a proceedure, or function
of type void returns no output.  What Pascal calls a function differs
in that it does return output.  In LPC, the most trivial, correct
function is:

.. code-block:: c

    void do_nothing() { }

This function accepts no input, performs no instructions, and returns no
value.

There are three parts to every properly written LPC function:

  1. The declaration
  2. The definition
  3. The call

Like with variables, functions must be declared.  This will allow the
driver to know (1) what type of data the function is returning as output,
and (2) how many input(s) and of what type those input(s) are. The
more common word for input is parameters. 

A function declaration therefore consists of:

.. code-block:: c

    type name(parameter1, parameter2, ..., parameterN);

The declaration of a function called ``drink_water()`` which accepts a string as
input and an int as output would thus look like this:

.. code-block:: c

   int drink_water(string str);

where str is the name of the input as it will be used inside the function.

The function definition is the code which describes what the function actually does with the input sent to it.  
The call is any place in other functions which invokes the execution of the function in question.  
For two functions ``write_vals()`` and ``add()``, you thus might have the following bit of code:

.. code-block:: c

   /* This is a comment block, it's purely for the developer, the driver does not care.
      First, function declarations.  They usually appear at the beginning
      of object code. 
    */
   void write_vals();
   int add(int x, int y);

   /* Next, the definition of the function write_vals().  We assume that
      this function is going to be called from outside the object
    */
   void write_vals()
   {
       int x;

       /*N Now we assign x the value of the output of add() through a call */
       x = add(2, 2);
       write(x+"\n");
   }

   /* Finally, the definition of add() */
   int add(int x, int y)
   {
       return (x + y);
   }

Remember, it does not matter which function definition appears first in the code.  This is because 
functions are not executed consecutively.  Instead, functions are executed as called.  The only 
requirement is that the declaration of a function appear before its definition and before the
definition of any function which makes a call to it. In the above example both functions are
declared at the top making the order irrelevant. If you do not want to declare them, make sure
a function only calls functions defined above.

4.3 Efuns
---------

Perhaps you have heard people refer to efuns.  They are externally defined functions.  Namely, 
they are defined by the MUD driver.  If you have played around at all with coding in LPC, you 
have probably found some expressions you were told to use like ``this_player()``,
``write()``, ``say()``, ``this_object()``, etc. look a lot like functions. That is because 
they are efuns. The value of efuns is that they are much faster than LPC functions,
since they already exist in the binary form the computer understands.

.. note::
   
   Notice, that ``this_player()`` is never used in LIMA, but in many other MUDs. We use ``this_body()``
   in LIMA. 

In the function ``write_vals()`` above, two functions calls were made.  The first was to the 
functions ``add()``, which you declared and defined.  The second call, however, was to a function
called ``write()``, and efun.  The driver has already declared and defined this function for you. 
You needs only to make calls to it.

Efuns are created to hanldle common, every day function calls, to handle input/output to the 
internet sockets, and other matters difficult to be dealt with in LPC.  They are written in C++
for FluffOS in the game driver and compiled along with the driver before the MUD comes up, 
making them much faster in execution.  But for your purposes, efun calls are just like calls
made to your functions. Still, it is important to know two things of any efun: 

  1. What return type does it have, and 
  2. what parameters of what types does it take.

Information for LIMA on this is documented on https://www.fluffos.info/efun/ and other pages 
on that website. The documentation is also available inside LIMA for your easy reference. It
is automatically updated when you rebuild lima on install.


.. code-block:: c

   void write(mixed str);

(See https://www.fluffos.info/efun/interactive/write.html)

This tells you an appropriate call to write expects no return value and
passes a single parameter of type mixed. The only reason this is a mixed type is that it can
be both a string or an integer (that will than be converted into a string).

4.4 Defining your own functions
-------------------------------

Although ordering your functions within the file does not matter, ordering
the code which defines a function is most important.  Once a function
has been called, function code is executed in the order it appears
in the function definition.  In ``write_vals()`` above, the instruction:
    
.. code-block:: c

   x = add(2, 2);

Must come before the ``write()`` efun call if you want to see the appropriate
value of ``x`` used in ``write()``.  

With respect to values returned by function, this is done through the "return"
instruction followed by a value of the same data type as the function.  In
``add()`` above, the instruction is "return (x+y);", where the value of ``(x+y)``
is the value returned to ``write_vals()`` and assigned to ``x``.  On a more
general level, "return" halts the execution of a function and returns
code execution to the function which called that function. In addition,
it returns to the calling function the value of any expression that follows.

To stop the execution of a function of type void out of order, use
"return"; without any value following.  Once again, remember, the data
type of the value of any expression returned using "return" MUST be the
same as the data type of the function itself.

.. note::

   You can stop the execution and throw an error using the ``error()`` efun.
   This is typically useful in the mudlib, but not suitable for players.
   See more at: https://www.fluffos.info/efun/system/error.html


4.5 Chapter Summary
-------------------

The files which define LPC objects are made of of functions.  Functions, in
turn, are made up of three parts:

    1. The declaration
    2. The definition
    3. The call

Function declarations generally appear at the top of the file before any
defintions, although the requirement is that the declaration must appear
before the function definition and before the definition of any function
which calls it.

Function definitions may appear in the file in any order so long as they
come after their declaration.  In addition, you may not define one function
inside another function.

Function calls appear inside the definition of other functions where you
want the code to begin execution of your function.  They may also appear
within the definition of the function itself, but this is not recommended
for new coders, as it can easily lead to infinite loops.

The function definition consists of the following in this order:

    1. function return type
    2. function name
    3. opening ( followed by a parameter list and a closing )
    4. an opening { instructing the driver that execution begins here
    5. declarations of any variables to be used only in that function
    6. instructions, expressions, and calls to other functions as needed
    7. a closing } stating that the function code ends here and, if no
       "return" instruction has been given at this point (type void functions
       only), execution returns to the calling function as if a r"return"
       instruction was given

The trivial function would thus be:

.. code-block:: c

   void do_nothing() {}

since this function does not accept any input, perform any instructions, or
return any output.

Any function which is not of type void MUST return a value of a data type
matching the function's data type.

Each driver has a set of functions already defined for you called efuns
These you need neither need to declare nor define since it has already
been done for you.  Furthermore, execution of these functions is faster
than the execution of your functions since efuns are in the driver.
In addition, each mudlib has special functions like efuns in that they
are already defined and declared for you, but different in that they
are defined in the mudlib and in LPC.  They are called simul_efuns, or
simulated efuns.  You can find out all about each of these as they are
listed on their respective websites. In addition many
MUDs have a command called "man", "apropos" or a "help" command which allows you
simply to call up the info files on them.

.. note::

   Some drivers may not require you to declare your functions, and some
   may not require you to specify the return type of the function in its
   definition.  Regardless of this fact, you should never omit this information
   for the following reasons:
    
    1. It is easier for other people (and you at later dates) to read your
       code and understand what is meant.  This is particularly useful
       for debugging, where a large portion of errors (outside of misplaced
       parentheses and brackets) involve problems with data types (Ever
       gotten "Bad arg 1 to foo() line 32"?).
    2. It is simply considered good coding form.

.. disqus::


5 The Basics of Inheritance
===========================

5.1 Review
----------

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

5.2 Object oriented programming
-------------------------------

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

5.3 How inheritance works
-------------------------
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

5.4 Chapter summary
-------------------
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


6 Variable Handling
===================

6.1 Review
----------

By now you should be able to code some simple objects using your muds standard
object library.  Inheritance allows you to use functions defined in those
objects without having to go and define yourself.  In addition,
you should know how to declare your own functions.  This
chapter will teach you about the basic elements of LPC which will allow you to
define your own functions using the manipulation of variables.

6.2 Values and objects
----------------------

Basically, what makes objects on the MUD different are two things:

   1. Some have different functions
   2. All have different values

Now, all player objects have the same functions.  They are therefore
differentiated by the values they hold.  For instance, the player
named "Forlock" is different from "Cartesius" *at least* in that they
have different values for the variable ``name``, those being
"cartesius" and "forlock".

Therefore, changes in the game involve changes in the values of the objects
in the game. Functions are used to name specific process for manipulating
values.  For instance, the ``setup()`` function is the function whose
process is specifically to initialize the values of an object.
Within a function, it is specifically things called instructions which are
responsible for the direct manipulation of variables.

6.3 Local and global variables
------------------------------

Like variables in most programming language, LPC variables may be declared
as variables "local" to a specific function, or "globally" available
to all functions. Local variables are declared inside the function which
will use them.  No other function knows about their existence, since
the values are only stored in memory while that function is being executed.
A global variable is available to any function which comes after its
declaration in the object code. Since global variables take up RAM for
the entire existence of the object, you should use them only when
you need a value stored for the entire existence of the object.

Have a look at the following 2 bits of code:

.. code-block:: c

   int x;

   int query_x() { return x; }
   void set_x(int y) { x = y; }

.. code-block:: c

   void set_x(int y) 
   {
       int x;

       x = y;
       write("x is set to x"+x+" and will now be forgotten.\n");
   }

In the first example, ``x`` is declared outside of any functions, and therefore
will be available to any function declared after it.  In that example,
``x`` is a global variable.

In the second example, ``x`` is declared inside the function ``set_x()``.  It
only exists while the function ``set_x()`` is being executed. Afterwards,
it ceases to exist. In that example, ``x`` is a local variable.

6.4 Manipulating the values of variables
----------------------------------------

Instructions to the driver are used to manipulate the values of variables.
An example of an instruction would be:

.. code-block:: c

   x = 5;

The above instruction is self-explanatory.  It assigns to the variable
``x`` the value 5. However, there are some important concepts in involved
in that instruction which are involved in instructions in general.
The first involves the concept of an expression. An expression is
any series of symbols which have a value.  In the above instruction,
the variable ``x`` is assigned the value of the expression 5.  Constant
values are the simplest forms in which expressions can be put.  A constant
is a value that never changes like the int 5 or the string "hello".
The last concept is the concept of an operator.  In the above example,
the assignment operator = is used.

There are however many more operators in LPC, and expressions can get
quite complex.  If we go up one level of complexity, we get:

.. code-block:: c

   y = 5;
   x = y +2;

The first instruction uses the assignment operator to assign the value
of the constant expression 5 to the variable y.  The second one
uses the assignment operator to assign to x the value of the expression
``(y+2)`` which uses the addition operator to come up with a value which
is the sum of the value of y and the value of the constant expression 2.

Sound like a lot of hot air?

In another manner of speaking, operators can be used to form complex
expressions. In the above example, there are two expressions in the
one instruction ``x = y + 2;``:

    1. The expression ``y+2``
    2. The expression ``x = y + 2``

As stated before, all expressions have a value.  The expression
``y+2`` has the value of the sum of ``y`` and 2 (here, 7);

The expression ``x = y + 2`` *also* has the value of 7.

So operators have to important tasks:

    1. They *may* act upon input like a function
    2. They evaluate as having a value themselves.

Now, not all operators do what 1 does.  The = operators does act upon
the value of 7 on its right by assigning that value to x.  The operator
+ however does nothing.  They both, however, have their own values.

6.5 Complex expressions
-----------------------

As you may have noticed above, the expression ``x = 5`` *itself* has a value
of 5.  In fact, since LPC operators themselves have value as expressions,
they can allow you to write some really convoluted looking nonsense like:

.. code-block:: c

   i = ( (x=sizeof(tmp=users())) ? --x : sizeof(tmp=children("/std/monster"))-1)

.. note::

    Assigning to ``tmp`` the array returned by the efun ``users()``, then assign to ``x``
    the value equal to the number of elements to that array.  If the value
    of the expression assigning the value to ``x`` is true (not 0), then assign
    ``x`` by 1 and assign the value of ``x-1`` to ``i``.  If ``x`` is false though,
    then set ``tmp`` to the array returned by the efun ``children()``, and then
    assign to ``i`` the value of the number of members in the array ``tmp`` -1.

Would you ever use the above statement? I doubt it.  However you might
see or use expressions similar to it, since the ability to consolidate
so much information into one single line helps to speed up the execution of
your code.  A more often used version of this property of LPC operators
would be something like:

.. code-block:: c

    x = sizeof(tmp = users());
    while(i--) write((string)tmp[i]->query_name()+"\n");

.. code-block:: c

    tmp = users();
    x = sizeof(tmp);
    for(i=0; tmp[i]->query_name()+"\n");

Things like ``for()``, ``while()``, arrays and such will be explained later.
But the first bit of code is more concise and it executed faster.

.. note::

    A detailed description of all basic LPC operators follows the chapter summary.

6.6 Chapter Summary
-------------------

You now know how to declare variables and understand the difference between
declaring and using them globally or locally.  Once you become familiar
with your driver's efuns, you can display those values in many different
ways.  In addition, through the LPC operators, you know how to change
and evaluate the values contained in variables.  This is useful of course
in that it allows you to do something like count how many apples have
been picked from a tree, so that once all apples have been picked, no
players can pick more.  Unfortunately, you do not know how to have
code executed in anything other than a linera fashion.  In other words,
hold off on that apple until the next chapter, cause you do not know
how to check if the apples picked is equal to the number of apples in the
tree.  

6.7 LPC operators
-----------------

This section contains a detailed listing of the simpler LPC operators,
including what they do to the values they use (if anything) and the value
that they have.

The operators described here are:

.. code-block:: c

     =    +    -    *    /    %    +=    -=    *=    /=    %=
     --    ++    ==    !=    >    <    >=    <=    !    &&    ||
     ->    ? :

Those operators are all described in a rather dry manner below, but it is best
to at least look at each one, since some may not behave *exactly* as
you think.  But it should make a rather good reference guide.

* **=** Assignment operator:

  Example: ``x = 5;``

  Value: the value of the variable on the *left* after its function is done
  explanation: It takes the value of any expression on the *right* and
  assigns it to the variable on the *left*.  Note that you must use
  a single variable on the left, as you cannot assign values to 
  constants or complex expressions.

* **+** Addition operator:
  
  Example: ``x + 7``

  Value: The sum of the value on the left and the value on the right
  
  Explanation: It takes the value of the expression on the right and
  adds it to the value of the expression on the left. For values
  of type int, this means the numerical sum. For strings,
  it means that the value on the right is stuck onto the value on
  the left ("ab" is the value of "a"+"b").  This operator does not
  modify any of the original values (i.e. the variable ``x`` from
  above retains its old value).

* **-** Subtraction operator:
  
  Example: ``x - 7``

  Value: the value of the expression on the left reduced by the right

  Explanation: Same characteristics as addition, except it subtracts.
  With strings: "a" is the value of "ab" - "b".

* ***** Multiplication operator:

  Example: ``x*7``
  
  Value and explanation: same as with adding and subtracting except
  this one performs the math of multiplication.

* **/** Division operator:
  
  Example: ``x/7``
  
  Value and explanation: see above.

* **+=** Additive assignment operator:
  
  Example: ``x += 5``

  Value: the same as x + 5
  
  Explanation: It takes the value of the variable on the left
  and the value of the expression on the right, adds them together
  and assigns the sum to the variable on the left.
  
  Example: if x = 2... x += 5 assigns the value
  7 to the variable x.  The whole expression has the value of 7.

* **-=** Subtraction assignment operator:
  
  Example: ``x-=7``
  
  Value: the value of the left value reduced by the right value
  
  Explanation: The same as += except for subtraction.

* ** \*= ** Multiplicative assignment operator:
  
  Example: ``x *= 7``
  
  Value: the value of the left value multiplied by the right
  
  Explanation: Similar to -= and += except for addition.

* **/=** Division assignment operator
  
  Example: ``x /= 7``
  
  Value: the value of the variable on the left divided by the right value
  
  Explanation: similar to above, except with division

* **++** Post/pre-increment operators
  
  Examples: ``i++`` or ``++i``
  
  Values: i++ has the value of i, ++i has the value of i+1
  
  Explanation: ++ changes the value of i by increasing it by 1.
  However, the value of the expression depends on where you
  place the ++.  ++i is the pre-increment operator.  This means
  that it performs the increment *before* giving a value.
  i++ is the post-ncrement operator.  It evalutes before incrementing
  i.  What is the point?  Well, it does not much matter to you at
  this point, but you should recognize what it means.

* **\-\-** Post/pre-decrement operators
  
  Examples: ``i--`` or ``--i``
  
  Values: i-- the value of i, --i the value of i reduced by 1
  
  Explanation: like ++ except for subtraction.

* **==** Equality operator
  
  Example: ``x == 5``
  
  Value: true or false (not 0 or 0)
  
  Explanation: it does nothing to either value, but it returns true if the 2 values are the same.
  It returns false if they are not equal.

* **!=** Inequality operator:
  
  Example: ``x != 5``
  
  Value: true or false
  
  Explanation returns true if the left expression is not equal to the right
  expression.  It returns fals if they are equal

* **>** greater than operator
  
  Example: ``x > 5``
  
  Value: true or false
  
  Explanation: true only if x has a value greater than 5
  false if the value is equal or less

* **<** Less than operator

* **>=** Greater than or equal to operator

* **<=** Less than or equal to operator
  
  Examples: ``x < y    x >= y    x <= y``
  
  Values: true or false
  
  Explanation: similar as to > except

    |  < true if left is less than right
    |  >= true if left is greater than *or equal to* right
    |  <= true if the left is less than *or equal to* the right

* **&&** Logical and operator:

* **||** Logical or operator:
  
  Examples: ``x && y      x || y``
  
  Values: true or false
  
  Explanation: If the right value and left value are non-zero, && is true.
  If either are false, then && is false.
  For ||, only one of the values must be true for it to evaluate
  as true.  It is only false if both values indeed
  are false

* **!** Negation operator:
  
  Example: ``!x``
  
  Value: true or false
  
  Explanation: If x is true, then !x is false. If x is false, !x is true.

A pair of more complicated ones that are here just for the sake of being
here.  Do not worry if they utterly confuse you.

* **->** The call other operator:
  
  Example: ``this_body()->query_name()``
  
  Value: The value returned by the function being called
  
  Explanation:  It calls the function which is on the right in the object
  on the left side of the operator.  The left expression *must* be
  an object, and the right expression *must* be the name of a function.
  If not such function exists in the object, it will return 0 (or
  more correctly, undefined).

* **? :**  Conditional operator
  
  Example: ``x ? y : z``
  
  Values: in the above example, if x is try, the value is y
  if x is false, the value of the expression is z
  
  Explanation: If the leftmost value is true, it will give the expression as
  a whole the value of the middle expression.  Else, it will give the
  expression as a whole the value of the rightmost expression.

.. note::

   A note on equality:  A very nasty error people make that is VERY difficult
   to debug is the error of placing = where you mean ==.  Since
   operators return values, they both make sense when being evaluated.
   In other words, no error occurs.  But they have very different values.  For example:
 
       ``if(x == 5)    if(x = 5)``

   The value of x == 5 is true if the value of x is 5, false othewise.
   The value of x = 5 is 5 (and therefore always true).
   The if statement is looking for the expression in () to be either true or false,
   so if you had = and meant ==, you would end up with an expression that is
   always true.  And you would pull your hair out trying to figure out
   why things were not happening like they should 😊

.. disqus::


7 Flow Control
==============

7.1 Review of variables
-----------------------

Variables may be manipulated by assigning or changing values with the
expressions =, +=, -=, ++, --.  Those expressions may be combined with
the expressions -, +, \*, /, %.  However, so far, you have only been
shown how to use a function to do these in a linear way.  For example:
 
.. code-block:: c

   int hello(int x) 
   {
       x--;
       write("Hello, x is "+x+".\n");
       return x;
   }
 
is a function you should know how to write and understand.  But what
if you wanted to write the value of ``x`` only if ``x = 1``?  Or what if
you wanted it to keep writing x over and over until ``x = 1`` before
returning?  LPC uses flow control in exactly the same way as C and C++.

7.2 The LPC flow control statements
-----------------------------------

LPC uses the following expressions:
 
.. code-block:: c

   if(expression) instruction;
 
   if(expression) instruction;
   else instruction;
 
   if(expression) instruction;
   else if(expression) instruction;
   else instruction;
 
   while(expression) instruction;
 
   do { instruction; } while(expression);
 
   switch(expression) 
   {
       case (expression): instruction; break;
       default: instruction;
   }

   foreach(type in array)
   {
     instruction;
   }

   foreach(type, type in mapping)
   {
     instruction;
   }


Before we discuss these, first something on what is meant by expression and
instruction.  An expression is anything with a value like a variable,
a comparison (like ``x > 5``, where if ``x`` is 6 or more, the value is 1, else the
value is 0), or an assignment(like ``x += 2``).  An instruction can be any
single line of lpc code like a function call, a value assignment or
modification, etc.
 
You should know also the operators &&, ||, ==, !=, and !.  These are the
logical operators.  They return a nonzero value when true, and 0 when false.
Make note of the values of the following expressions:
 
   |  (1 && 1) value: 1   (1 and 1)
   |  (1 && 0) value: 0   (1 and 0)
   |  (1 || 0) value: 1   (1 or 0)
   |  (1 == 1) value: 1   (1 is equal to 1)
   |  (1 != 1) value: 0   (1 is not equal to 1)
   |  (!1) value: 0       (not 1)
   |  (!0) value: 1       (not 0)
 
In expressions using &&, if the value of the first item being compared
is 0, the second is never tested even.  When using ||, if the first is
true (1), then the second is not tested.
 
7.3 if()
--------
The first expression to look at that alters flow control is if().  Take
a look at the following example:
 
.. code-block:: c

   1 void ``test()``
   2 {
   3     int x;
   4
   5     x = random(100);
   6     if(x > 50) new ("/domains/std/ammo/11mm_pistol")->move(this_object());
   7 }
 
The line numbers are for reference only.

In line 3, of course we declare a variable of type int called ``x``.  Line 4
is aethetic whitespace to clearly show where the declarations end and the
function code begins.  The variable ``x`` is only available to the function
``test()``.

Line 5 uses the driver efun ``random()`` to return a random number between
0 and the parameter minus 1.  So here we are looking for a number between
0 and 99.

In line 6, we test the value of the expression ``(x>50)`` to see if it is true
or false.  If it is true, then it makes a call to the ``new()`` function, create 
an 11mm pistol ammo clip and move it to this room - it will be on the floor.  
If it is false, the call to ``new()`` is never executed.

In line 7, the function returns driver control to the calling function
(the driver itself in this case) without returning any value.
 
If you had wanted to execute multiple instructions instead of just the one,
you would have done it in the following manner:
 
.. code-block:: c

   if(x>50) 
   {
    new ("/domains/std/ammo/11mm_pistol")->move(this_object());
    if(!present("beggar", this_object())) clone_beggar();
   }

Notice the {} encapsulates the instructions to be executed if the test
expression is true.  In the example, again we call the ``new()`` function
which clones the ammo.  Next, there is another ``if()`` expression that tests the
truth of the expression ``(!present("beggar",this_object()))``.  

The ``!`` in the test expression changes the truth of the expression which follows it.  In
this case, it changes the truth of the efun ``present()``, which will return
the object that is a beggar if it is in the room (``this_object()``), or it
will return 0 if there is no beggar in the room.  So if there is a beggar
still living in the room, (``present("beggar", this_object())``) will have
a value equal to the beggar object (data type is then *object*), otherwise it will
be 0.  The ! will change a 0 to a 1, or any nonzero value (like the
beggar object) to a 0.  Therefore, the expression
(``!present("beggar", this_object())``) is true if there is no beggar in the
room, and false if there is.  So, if there is no beggar in the room,
then it calls the function you define in your room code that makes a
new beggar and puts it in the room. (If there already is a beggar in the room,
we do not want to add yet another one)
 
Of course, ``if()``'s often comes with ands or buts.  In LPC, the formal
reading of the ``if()`` statement is:
 
.. code-block:: c

   if(expression) { set of intructions }
   else if(expression) { set of instructions }
   else { set of instructions }
 
This means:
 
If expression is true, then do these instructions.
Otherise, if this second expression is true, do this second set.
And if none of those were true, then do this last set.
 
You can have ``if()`` alone:
 
.. code-block:: c

   if(x>5) write("Foo,\n");
 
with an else ``if()``:
 
.. code-block:: c

   if(x > 5) write("X is greater than 5.\n");
   else if(x >2) write("X is less than 6, but greater than 2.\n");
 
with an else:
 
.. code-block:: c

   if(x>5) write("X is greater than 5.\n");
   else write("X is less than 6.\n");
 
or the whole lot of them as listed above.  You can have any number of
else ``if()``'s in the expression, but you must have one and only one
``if()`` and at most one else.  Of course, as with the beggar example,
you may nest ``if()`` statements inside ``if()`` instructions.

For example:

.. code-block:: c

       if(x>5) 
       {
           if(x==7) write("Lucky number!\n");
           else write("Roll again.\n");
       }
       else write("You lose.\n");
 

7.4 The statements: while() and do {} while()
---------------------------------------------
Prototype:

.. code-block:: c

   while(expression) { set of instructions }
   do { set of instructions } while(expression);
 
These allow you to create a set of instructions which continue to
execute so long as some expression is true.  Suppose you wanted to
set a variable equal to a player's level and keep subtracting random
amounts of either money or hp from a player until that variable equals
0 (so that player's of higher levels would lose more).  You might do it
this way:
 
.. code-block:: c
   :linenos:

   int x;
   
   x = (int)this_body()->query_level();  /* this has yet to be explained */
   while(x > 0) 
   {
   if(random(2)) this_body()->add_money("gold", random(50));
   else this_body()->hurt_us("head",random(10));
   x--;
   }
 
Line 1, definition of ``x``. Line 3 has the expression ``this_body()->query_level()``
to fetch the level of the player. In line 4, we start a loop that executes so long as ``x`` 
is greater than 0. 

In line 6-7, we add anywhere between 0 and 49 coins to the player, but if instead it returns 0, 
we call the hurt_us() function in the player which reduces the player's hit points anywhere between 
0 and 9 hp on the limb called "head". In line 8, we reduce ``x`` by 1.

At line 0, the execution comes to the end of the while() instructions and
goes back up to line 4 to see if x is still greater than 0.  This
loop will keep executing until x is finally less than 1.
 
You might, however, want to test an expression *after* you execute some
instructions.  For instance, in the above, if you wanted to execute
the instructions at least once for everyone, even if their level is
below the test level:
 
.. code-block:: c
   :linenos:

    int x;
 
    x = (int)this_player()->query_level();
    do 
    {
        if(random(2)) this_body()->add_money("gold", random(50));
        else this_body()->hurt_us("head",random(10));
        x--;
    } while(x > 0);
 
This is a rather bizarre example, being as few muds have level 0 players.
And even still, you could have done it using the original loop with
a different test.  Nevertheless, it is intended to show how a ``do{} while()``
works.  As you see, instead of initiating the test at the beginning of the
loop (which would immediately exclude some values of ``x``), it tests after
the loop has been executed.  This assures that the instructions of the loop
get executed at least one time, no matter what ``x`` is.

.. note::
    
    The ``do{} while();`` construct is a rather arcane example from 1993, and not something
    you would encounter in a modern mudlib.


7.5 for() loops
---------------

Prototype:

.. code-block:: c

   for(initialize values ; test expression ; instruction) 
   { 
     instructions 
   }
 
Initialize values:

This allows you to set starting values of variables which will be used
in the loop.  This part is optional.
 
Test expression:

Same as the expression in ``if()`` and ``while()``.  The loop is executed
as long as this expression (or expressions) is true. You must have a
test expression.
 
Instruction:
An expression (or expressions) which is to be executed at the end of each
loop. This is optional.
 
.. note::

   ``for(;expression;) {}`` IS EXACTLY THE SAME AS ``while(expression) {}``
 
Example:
 
.. code-block:: c
   :linenos:

   for(int x= this_player()->query_level(); x>0; x--) 
   {
       if(random(2)) this_body()->add_money("gold", random(50));
       else this_body()->hurt_us("head",random(10));
   }
 
This ``for()`` loop behaves *exactly* like the ``while()`` example.
Additionally, if you wanted to initialize 2 variables:
 

7.6 The statement: switch()
---------------------------

Prototype:

.. code-block:: c

   switch(expression) 
   {
      case constant: instructions
      case constant: instructions
      ...
      case constant: instructions
      default: instructions
   }

This is functionally much like ``if()`` expressions, and much nicer to the
CPU, however most rarely used because it looks so damn complicated.
But it is not.
 
First off, the expression is not a test.  The cases are tests.  A English
sounding way to read:
 
.. code-block:: c
   :linenos:

   int x;
   
   x = random(5);
   switch(x) {
       case 1: write("X is 1.\n");
       case 2: x++;
       default: x--;
   }
   write(x+"\n");
 
Would be:
 
   |  Set variable x to a random number between 0 and 4.
   |  In case 1 of variable x write its value add 1 to it and subtract 1.
   |  In case 2 of variable x, add 1 to its value and then subtract 1.
   |  In other cases subtract 1.
   |  Write the value of x.
 
The ``switch(x)`` statement, basically tells the driver that the variable ``x`` is the value
we are trying to match to a case. Once the driver finds a case which matches, 
that case *and all following cases* will be acted upon.  You may break out of the switch statement
as well as any other flow control statement with a break instruction in
order only to execute a single case.  But that will be explained later.

The default statement is one that will be executed for any value of
x so long as the switch() flow has not been broken.  You may use any
data type in a switch statement:
 
.. code-block:: c

   string name;
 
   name = (string)this_player()->query_name();
   switch(name) 
   {
       case "cartesius": write("You borg.\n");
       case "flamme":
       case "forlock":
       case "shadowwolf": write("You are a Nightmare head arch.\n");
       default: write("You exist.\n");
   }
 
For "cartesius", you would see:

  |  You borg.
  |  You exist.
 
Flamme, Forlock, or Shadowwolf would see:

  |  You are a Nightmare head arch.
  |  You exist.
 
Everyone else would see:

  |  You exist.
 

7.7 foreach() statement
-----------------------

The ``foreach()`` statement comes in two forms, and in specialized in interactions
over arrays or mappings. A simple example of a ``foreach()`` could be:

.. code-block:: c
   :linenos:

   foreach(object user in users())
   {
     tell(user,"Hello there!");
     write("We just said \"Hello\" to "+user->query_name());
   }

In this example we define ``object user`` as part of the ``foreach()``, iterate over
the array of users in the order given, and call ``user->query_name()`` on each of the
objects in the array. In line 4, we use the ``tell()`` function (a simulated efun (sefun), 
more on those later), and in line 5 we write to the current user a piece of text.

The other option is to use it to iterate over mappings, here is a short example of how that is done.
A mapping is basically a hash map with keys and value pairs. An example could be:

   |  cartesius : 1
   |  tsath : 2
   |  forlock: 3

This can be expressed as a single mapping as:

.. code-block:: c

   mapping m;

   m=(["cartesius":1,"tsath":2,"forlock":3]);

The names being the keys, and the numbers being the values. Values can be strings, objects, integers,
mappings, arrays and other types. A foreach for the mapping above would look like:

.. code-block:: c
   :linenos:

   mapping m = (["cartesius":1,"tsath":2,"forlock":3]);
   
   foreach(string name, int val in m)
   {
      tell(find_body(name),"Hello there!");
      write("We just said \"Hello\" to "+name+", value is: "+val);
   }

Notice how the structure of the mapping is reflected in the types defined in the
``foreach()``, so ``string name`` since our key is a string, and ``int val`` since our values
are integers.

.. note::

    The ``foreach()`` function can nest other ``foreach()`` loops to deal with mappings
    with arrays, etc. This is widely used in LIMA as it is both effective and easy to read.

7.8 Altering the flow of functions and flow control statements
--------------------------------------------------------------

The following instructions alter the natural flow of things as described above:

  * ``return``
  * ``continue``
  * ``break``
 
First of all, ``return`` no matter where it occurs in a function, will cease the execution of that
function and return control to the function which called the one the return statement is in. If 
the function is NOT of type void, then a value must follow the return statement, and that value 
must be of a type matching the function.  An absolute value function would look like this:
 
.. code-block:: c

   int absolute_value(int x) 
   {
       if(x>-1) return x;
       else return -x;
   }
 
In the second line, the function ceases execution and returns to the calling function because the 
desired value has been found if x is a positive number.

.. note::

    The ``absolute_value()`` function above is not something you would do, since we now have an efun
    called ``abs()`` that does the same.

``continue`` is most often used in ``for()``, ``foreach()``, and ``while()`` statements.  
It serves to stop the execution of the current loop and send the execution back
to the beginning of the loop.  For instance, say you wanted to avoid
division by 0:
 
.. code-block:: c
   :linenos:

   int x= 4;
   while( x > -5) 
   {
       x--
       if(!x) continue;
       write((100/x)+"\n");
   }
   write("Done.\n")
 
You would see the following output:

  |  33
  |  50
  |  100
  |  -100
  |  -50
  |  -33
  |  -25
  |  Done.

To avoid an error, it checks in each loop to make sure x is not 0.
If x is zero, then it starts back with the test expression without
finishing its current loop. ``continue`` is typically used to skip
something while in a loop, e.g. not healing the player who is the vampire.
 
In a for() expression:

.. code-block:: c
   :linenos:

    for(x=3; x>-5; x--) 
    {
       if(!x) continue;
       write((100/x)+"\n");
    }
    write("Done.\n");

It works much the same way.  Note this gives exactly the same output
as before. At ``x=1``, it tests to see if ``x`` is zero, it is not, so it
writes 100/x, then goes back to the top, subtracts one from ``x``, checks to
see if it is zero again, and it is zero, so it goes back to the top
and subtracts 1 again.
 
Last, there is ``break``. This one ceases the function of a flow control statement.  No matter
where you are in the statement, the control of the program will go
to the end of the loop.  So, if in the above examples, we had
used break instead of continue, the output would have looked like this:
 
33
50
100
Done.
 
continue is most often used with the for() and while() statements.
break however is mostly used with switch()
 
.. code-block:: c
   :linenos:

   switch(name) 
   {
       case "cartesius": write("You are borg.\n"); break;
       case "flamme": write("You are flamme.\n"); break;
       case "forlock": write("You are forlock.\n"); break;
       case "shadowwolf": write("You are shadowwolf.\n"); break;
       default: write("You will be assimilated.\n");
   }
 
This functions just like:
 
.. code-block:: c
   :linenos:

   if(name == "cartesius") write("You are borg.\n");
   else if(name == "flamme") write("You are flamme.\n");
   else if(name == "forlock") write("You are forlock.\n");
   else if(name == "shadowwolf") write("You are shadowwolf.\n");
   else write("You will be assimilated.\n");
 
Except the switch statement is much better on the CPU.
If any of these are placed in nested statements, then they alter the
flow of the most immediate statement.

.. note::

    Not having a ``break`` statement inside a specific ``case`` in a ``switch``
    can be quite useful. Sometimes you do want to apply both that case and the one
    after the case. This is sometimes referred to as "falling through" the case
    statement.

7.9 Chapter summary
-------------------

This chapter covered one hell of a lot, but it was stuff that needed to
be seen all at once.  You should now completely understand ``if()``, ``for()``,
``foreach()``, ``while()``, and ``switch()``, as well as how to alter their flow
using return, continue, and break.  Effeciency says if it can be done in
a natural way using ``switch()`` instead of a lot of ``if()`` else ``if()``'s, then
by all means do it.  You were also introduced to the idea of calling
functions in other objects.  That however, is a topic to be detailed later.
You now should be completely at ease writing simple rooms (if you have
read your mudlib's room building document), simple monsters, and
other sorts of simple objects.

.. disqus::


8 The data type "object"
========================

8.1 Review
----------

You should now be able to do anything so long as you stick to calling
functions within your own object. You should also know, that at the
bare minimum you can get the ``setup()`` (or ``create()``) function in your object
called to start just by loading it into memory. Note that neither of these
functions MUST be in your object. The driver checks to see if the
function exists in your object first.  If it does not, then it does not
bother. You are also acquainted with the data types void, int, and string.
 
8.2 Objects as data types
-------------------------

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
 
8.3 The efun: this_object()
---------------------------

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

8.4 Calling functions in other objects
--------------------------------------

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

9 Verbs and interactions
========================

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

Use the :doc:`parse <../command/parse>` command (in front of the normal verb syntax) to see the 
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


