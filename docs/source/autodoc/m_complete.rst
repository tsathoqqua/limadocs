m_complete
==========

string* complete(string partial, string* potentials)
----------------------------------------------------

return an array of all strings that would be valid completions.

string* case_insensitive_complete(string partial, string* potentials)
---------------------------------------------------------------------

same as complete, but upper and lower case are ignored.

string *find_best_match_or_complete(string partial, string *potentials)
-----------------------------------------------------------------------

 returns only that.

mixed complete_user(string name)
--------------------------------

user name, or an array of strings on a partial match, or 0 on no match.