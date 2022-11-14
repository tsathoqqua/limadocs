*********
Messaging
*********

The `M_MESSAGES module <../module/m_messages.html>`_ handles messaging with the correct grammar for multiple parties. 
It uses the same syntax as the `feelings/souls/emotes <../player_command/feelings.html>`_ on the MUD and using `Command: showemote <../command/showemote.html>`_ to see how emotes are done is a good way of picking up the idea.

Tokens::

    $N - "You" or "Tsath"
    $n - "you" or "Tsath"

    $T - Some other indivial, "Gesslar" or "You"
    $t - Some other indivial, "Gesslar" or "you"

    $P - Possessive, "Your", "His", "Her", "Its"
    $p - Possessive, "your", "his", "her", "its"

    $O - Object or item, "Map", "It"
    $o - Object or item, "map", "it"

    $R - Reflexive, "Yourself", "Himself", "Herself", "Itself"
    $r - Reflexive, "yourself", "himself", "herself", "itself"

    $v - verb

Do not worry, these will make more sense after a few examples.

simple_action()
===============
simple_action() is the simplest of the bunch, as it just creates one message for the body doing the action, and one for the rest.

Example::

    "You say: This is a test."
    "Tsath says: This is a test."

We can see 'You' turns into 'Tsath' and 'say' into 'says', and simple action handles this for us if we use the tokens::

   $N $vsay: $O

Using $O instead of $o here, would capitalize whatever is being said. To test this using the wizard shell try::

    @this_body()->simple_action("$N $vsay, \"$O\"", "this is a test")

To further improve, we can use the shortcut for *this_body()* in the shell called *.me* and punctuate the string if needed::

    @.me->simple_action("$N $vsay, \"$O\"", punctuate("this is a test"));

Simple action also works with objects as the objects do not need to receive special messages. For this example clone a couple of objects to your inventory::

    clone /domains/std/map
    clone /domains/std/camera

You can use the `inventory command <..//player_command/inventory.html>`_ or `'scan me' <../commands/scan.html>`_ to verify that you have the objects.

Now try something like::

    @.me->simple_action("$N $vput the $o into the $o1.",.me:map, .me:camera)

And you will get::

   You put the map into the polaroid camera.

A couple of new things here: The shell allows you to refer to objects in the inventory of objects by using *something:somethingelse* in this case *.me* being *this_body()* and *map* as the id we're looking for. Also, since you are now sending *simple_action()* more objects, you can use $o to refer to the first object, and $o1 to refer to the second one. This also means that "$t" is actually just an alias for "$n1".

A few wizard shell shortcuts and the corresponding LPC::

 	.me   -> this_body()
	.rust -> find_body("rust")
	.here -> environment(this_body())
	.xxx:shell -> xxx->query_shell_ob()
	.xxx:link ->  xxx->query_link()
	.xxx:foo -> present("xxx", foo)
	./wiz/rust/blah -> load_object("/wiz/rust/blah")

targetted_action()
==================
This is used for doing something to someone else, while sending the correct message to third parties.

Example::
      "You give Gesslar a hug."
      "Tsath gives you a hug."
      "Tsath gives Gesslar a hug."

To generate something like this we can use the $t token::

    @.me->targetted_action("$N $vgive $t a hug.",.here:greeter)

The *.here:greeter* works in the Wizroom where the LIMA Mudlib Greeter is, otherwise use another beign in your environment.

*targetted_action()* can also combine a target with a list of objects, like this::

    @.here:greeter->targetted_action("$N $vgive $t a hug and $p $o.",.me,.me:map)
