m_gettable
==========

void set_getmsg( string s )
---------------------------

an object.

string query_getmsg()
---------------------

an object.

void set_gettable( mixed g )
----------------------------

same effect as calling set_getmsg().

mixed get()
-----------

otherwise 0 or a string error message. 

void set_dropmsg( string s )
----------------------------

Set the error message that one gets when one tries to drop an object

string query_dropmsg()
----------------------

returns the error message one gets when one tries to drop an object.

void set_droppable( int g )
---------------------------

same effect as calling set_dropmsg().

mixed drop()
------------

otherwise 0 or a string error message. 

int is_gettable()
-----------------

return one if an object can be taken.