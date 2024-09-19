Frequently asked questions
==========================

.. contents::
   :local:

..
  Frequently asked questions should be questions that actually got asked.
  Formulate them as a question and an answer.
  Consider that the answer is best as a reference to another place in the documentation.

Initial woes
------------


Why is IMUD_D complaining and how to fix it?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

IMUD_D requires a valid email set in ``/include/config.h`` under:

.. code-block:: c

   /* The administrator(s)' email address(es).
    * NOTE: This is required to be changed in order to have a working
    * I3/IMUD_D system. Must be changed for anything to work!
    */
   #define ADMIN_EMAIL "billg@microsoft.com"

Once that has been changed, log in to your MUD, and do 

   |  update \`IMUD_D\`

and it should then load.

Why am I getting "Too long evaluation. Execution aborted."?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is usually caused by your MUD host being a bit on the slow side, and slowing down
the call_out() rate the daemons use can help this. Go to ``/include/config.h``
and change this:

.. code-block:: c

   /* Delay factor for DAEMON call_outs(). 
    * Some daemons may be a bit too greedy for your machine causing:
    * "Too long evaluation. Execution aborted."
    * If you are getting these increase this number to 5, or 10.
    * Otherwise, enjoy your powerful machine, and keep it at 1.
    */
   #define TOO_GREEDY_DAEMONS 1

Change the 1 here to as low a number as will make the issue go away. You can try
5 or 10, and then possibly reduce it a bit once the errors stop nagging you.

Alternatively, upgrade your hosting to a bigger potato. 🥔

.. disqus::

