m_messages
==========

varargs string compose_message(object forwhom, string msg, object *who, 
  mixed *obs...)
---------------------------------------------------------------------------------------

Usually this routine is used through the higher level interfaces.

varargs string *action(object *who, mixed msg, mixed *obs...)
-------------------------------------------------------------

see: inform

void inform(object *who, string *msgs, mixed others)
----------------------------------------------------

message, or an array of people to recieve the 'other' message.

varargs void simple_action(mixed msg, mixed *obs...)
----------------------------------------------------

some objects

varargs void my_action(mixed msg, mixed *obs...)
------------------------------------------------

Generate and send a message that should only be seen by the person doing it

varargs void other_action(mixed msg, mixed *obs...)
---------------------------------------------------

Generate and send a message that should only be seen by others

varargs void targetted_action(mixed msg, object target, mixed *obs...)
----------------------------------------------------------------------

other objects)