msg_recipient
=============

int environment_can_hear()
--------------------------

Returns 1 if messages should propagate upwards to our environment.

receive_inside_msg(string msg, object *exclude, int message_type, 
			mixed other)
----------------------------------------------------------------------------------

Inside messages propogate upward and downward, but we have no downward

receive_outside_msg(string msg, object *exclude, int message_type,
			 mixed other)
-----------------------------------------------------------------------------------

Outside messages propogate downward, so just receive

receive_private_msg(string msg, int message_type, mixed other)
--------------------------------------------------------------

Receive a message sent just to us.