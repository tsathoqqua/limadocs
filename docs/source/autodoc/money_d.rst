money_d
=======

string singular_name(string name)
---------------------------------

e.g. lover case, without spaces and singular.

nomask int query_exchange_rate(string type)
-------------------------------------------

of units this currency is worth.

nomask string *query_currency_types()
-------------------------------------

Returns the names of the currencies that are valid within the mud

varargs nomask string *query_denominations(string type)
-------------------------------------------------------

or all available denominations

nomask int is_currency(string name)
-----------------------------------

tests if name is a currency

nomask int is_denomination(string name)
---------------------------------------

tests if name is a denomination

nomask string query_currency(string name)
-----------------------------------------

Returns the currency of a denomination

nomask string query_plural(string name)
---------------------------------------

Returns the plural name of a denomination

nomask float query_factor(string name)
--------------------------------------

Returns the exchange rate factor of a denomination

varargs nomask void add_currency(string type, string plural, int flag)
----------------------------------------------------------------------

name will be created too if flag is 0.

nomask void remove_currency(string type)
----------------------------------------

Removes a currency from the money system.

nomask void set_exchange_rate(string type, int rate)
----------------------------------------------------

Set the exchange rate (that is the value) of a currency

void add_denomination(string type, string name, string plural, float factor)
----------------------------------------------------------------------------

add a denomination to a currency

void remove_denomination(string name)
-------------------------------------

removes a denomination from a currency

nomask string denomination_to_string(int amount, string type)
-------------------------------------------------------------

create a string with correct use of plural from an amount of a denomination.

mapping calculate_denominations(float f_amount, string currency)
----------------------------------------------------------------

calculate denominations which add up to a certain amount.

varargs nomask string currency_to_string(mixed money, string currency)
----------------------------------------------------------------------

The output is only sorted if you specify the currency.

mapping *handle_subtract_money(object player, float f_amount, 
				     string type)
------------------------------------------------------------------------------------

consist of the denominations which were used.