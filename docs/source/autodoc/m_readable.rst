m_readable
==========

void set_text( string t )
-------------------------

If it is a filename, either absolute, or relative paths may be used.

mixed query_text()
------------------

See set_text()

int has_entries()
-----------------

See set_entries()

void set_entries(mapping e)
---------------------------

a simple string.

void add_entry(mixed name,mixed contents)
-----------------------------------------

See set_entries()

void remove_entry(string entry)
-------------------------------

See set_entry()

void clear_entries()
--------------------

Clear all of the entires.  Both pages and synonyms.

mixed query_entry(string entry)
-------------------------------

The string argument can be the actual entry name or a synonym of it.

string *list_entries()
----------------------

A list of all the readable entries on the object

mapping dump_entries()
----------------------

Raw dump of all the entry data.

void set_synonyms(mapping s)
----------------------------

the entry.

void set_entry_synonyms(mapping s)
----------------------------------

the entry.

void add_synonym(string syn,string entry)
-----------------------------------------

which it refers

void remove_synonym(string syn)
-------------------------------

Removes a synonym of an entry

string query_synonym(string syn)
--------------------------------

Return the entry that the synonym refers to

mapping dump_synonyms()
-----------------------

Return the mapping of all synonyms