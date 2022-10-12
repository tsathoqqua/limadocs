m_vendor
========

float set_cost_multiplicator(float m)
-------------------------------------
0

float selling_cost(float cost)
------------------------------

multiply the objects value with cost_mult to get the cost for selling

float buying_cost(float cost)
-----------------------------

override if you want a different way to determine cost

void add_sell(string file, int amt)
-----------------------------------

enables you to add items to the vendors stored_item's mapping

void set_sell(mapping items)
----------------------------

 with a mapping you can set many items into the vendor's to sell list

void add_sell_object(object ob)
-------------------------------

 adds a unique item to the vendor's stored_items mapping

void set_for_sale(mixed x)
--------------------------

If a single string is returned it will be used as error message.

void set_will_buy(mixed x)
--------------------------

argument. If a single string is returned it will be used as error message.

mixed set_currency_type(string type)
------------------------------------

Sets the type of currency the vendor will buy/sell in

mixed query_currency_type()
---------------------------

Queries the type of currency the vendor will buy/sell in

int query_items(string item, int flag)
--------------------------------------

If flag is set the it will show the long() too

void sell_stored_objects(string item, int number, int amount)
-------------------------------------------------------------

that he has stored away.

void set_unique_inventory(string str)
-------------------------------------

See set_all_unique to unique everything

void set_all_unique(int i)
--------------------------

is used.

int check_uniqueness(object ob)
-------------------------------

depending on destroyable(), set_all_unique() and is_unique().