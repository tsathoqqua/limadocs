money
=====

int query_amt_money(string type)
--------------------------------

of currency you have.

float query_amt_currency(string currency)
-----------------------------------------

currency you have.

void add_money(string type, int amount)
---------------------------------------

This is the function to call to add money to a person 

void subtract_money(string type, int amount)
--------------------------------------------

This is the function to call to substract money from a person 

string *query_currencies()
--------------------------

This function will return the current "types" of money you have

mapping query_money()
---------------------

This function will return the complete money mapping