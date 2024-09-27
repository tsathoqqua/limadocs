#################
Variable Handling
#################

Review
======

By now you should be able to code some simple objects using your muds standard
object library.  Inheritance allows you to use functions defined in those
objects without having to go and define yourself.  In addition,
you should know how to declare your own functions.  This
chapter will teach you about the basic elements of LPC which will allow you to
define your own functions using the manipulation of variables.

Values and objects
==================

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

Local and global variables
==========================

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

Manipulating the values of variables
====================================

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

Complex expressions
===================

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

Chapter Summary
---------------

You now know how to declare variables and understand the difference between
declaring and using them globally or locally.  Once you become familiar
with the efuns of your driver, you can display those values in many different
ways.  In addition, through the LPC operators, you know how to change
and evaluate the values contained in variables.  This is useful of course
in that it allows you to do something like count how many apples have
been picked from a tree, so that once all apples have been picked, no
players can pick more.  Unfortunately, you do not know how to have
code executed in anything other than a linear fashion.  In other words,
hold off on that apple until the next chapter, cause you do not know
how to check if the apples picked is equal to the number of apples in the
tree.  

LPC operators
=============

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
  i++ is the post-increment operator.  It evaluates before incrementing
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
  expression.  It returns fails if they are equal

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

   The value of x == 5 is true if the value of x is 5, false otherwise.
   The value of x = 5 is 5 (and therefore always true).
   The if statement is looking for the expression in () to be either true or false,
   so if you had = and meant ==, you would end up with an expression that is
   always true.  And you would pull your hair out trying to figure out
   why things were not happening like they should 😊



