m_blockexits
============

void add_block(string *exits...)
--------------------------------

'all' can be used to block all exits

void remove_block(string *exits...)
-----------------------------------

Removes the exits specified to the list of exits this object is blocking

void set_block_action(mixed ba)
-------------------------------

as args, and the move will be allowed only if the function returns 1.