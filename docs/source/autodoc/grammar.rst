grammar
=======

void set_gender(int x)
----------------------

set the objects gender.  0 == neuter, 1 == male, 2 == female

int query_gender()
------------------

query an object's gender

string query_gender_string()
----------------------------

Query the string representation of an objects gender

string query_pronoun()
----------------------

return the (subjective) pronoun of a object (he, she, it)

string query_objective()
------------------------

return the objective pronoun of an object (it, him, her)

string query_subjective()
-------------------------

return the subjective pronoun of an object (he, she, it)

string query_possessive()
-------------------------

return the possessive pronoun of an object (his, her, its)

string query_named_possessive()
-------------------------------

return the named possessive of an object (Foo's)

string query_reflexive()
------------------------

return the reflexive pronoun of an object (himself, herself, itself)