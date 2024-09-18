############
Flow Control
############

Review of variables
===================

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

The LPC flow control statements
===============================

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
 
if()
====
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
 

The statements: while() and do {} while()
=========================================
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


for() loops
===========

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
 

The statement: switch()
=======================

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
 

foreach() statement
===================

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

Altering the flow of functions and flow control statements
==========================================================

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

Chapter summary
===============

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

