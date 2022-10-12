bindings
========

nomask void init_charmode(mapping new_bindings, function d)
-----------------------------------------------------------

function to handle keystrokes not covered in the binding map.

nomask void bind(string sequence, function callback)
----------------------------------------------------

Add a key binding to a charmode shell.

nomask void unbind(string sequence)
-----------------------------------

Remove a key binding from a charmode shell.