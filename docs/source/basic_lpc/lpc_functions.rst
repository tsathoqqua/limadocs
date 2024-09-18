#########
Functions
#########
This chapter will teach you about functions, how to define them, how they work,
and how to call them.

Review
======

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

What is a function?
===================

Like math functions, LPC functions take input and return output.
Languages like Pascal distinguish between the concept of procedure abd
the concept of function.  LPC does not, however, it is useful to
understand this distinction.  What Pascal calls a procedure, LPC
calls a function of type void.  In other words, a procedure, or function
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

Efuns
=====

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

Efuns are created to handle common, every day function calls, to handle input/output to the 
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

Defining your own functions
===========================

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


Chapter Summary
===============

The files which define LPC objects are made of of functions.  Functions, in
turn, are made up of three parts:

    1. The declaration
    2. The definition
    3. The call

Function declarations generally appear at the top of the file before any
definitions, although the requirement is that the declaration must appear
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
are defined in the mudlib and in LPC.  They are called simul-efuns, or
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

