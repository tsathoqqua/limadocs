Preprocessor
############

In LPC, anything starting with # is handled by the preprocessor before the code is compiled. The 
preprocessor is a simple text substitution system that modifies the source code according to directives.

Here are some common preprocessor directives mentioned:

  - ``#define``: Defines a macro or constant, replacing occurrences of a symbol with its value.
  - ``#ifdef``: Conditional compilation; includes code if a symbol is defined.
  - ``#if``: Conditional compilation; includes code based on an expression.
  - ``#undef``: Undefines a previously defined macro or symbol.

The preprocessor handles these directives before FluffOS processes the code. This allows developers to:

   - Define constants or macros for reusable code
   - Conditionally include or exclude code based on configurations
   - Perform basic text substitutions

For example:

.. code-block:: c

   #define MAX_HEALTH 100

   int health = MAX_HEALTH;

   #ifdef DEBUG
     write("Debug mode enabled.");
   #endif

In this example, the preprocessor replaces MAX_HEALTH with 100 and includes the write statement 
only if DEBUG is defined.

Keep in mind that the preprocessor is a simple text substitution system, and its capabilities are 
limited compared to the full LPC language.

.. note::

    LIMA uses #define and #undef to set settings that modifies the mudlib in drastic ways.
    See ``/include/config.h`` and ``/include/combat_modules.h`` for two of the most import ones.
    Other configs in LIMA are in ``/include/config/*.h``, and they are modified using the
    :doc:`admtool <../command/admtool>`.