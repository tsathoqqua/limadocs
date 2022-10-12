book
====

void set_pages(mixed *page_data...)
-----------------------------------

page, a filename, or a function pointer that generates one of these.

void add_pages(string *page_data...)
------------------------------------

See set_pages()

void remove_pages(mixed *pagenum...)
------------------------------------

See set_pages()

string *list_pages()
--------------------

Return all of the pages in the book in an array.

string query_page(int pagenum)
------------------------------

Return the contents of the page specified.

void set_contents(mapping new_contents)
---------------------------------------

heading as the key and the page it starts with as the value (a string).

void add_contents(mapping additions)
------------------------------------

See set_contents

void remove_contents(string *headings...)
-----------------------------------------

See set_contents()

string query_content_page(string heading)
-----------------------------------------

Return the page number of the contents heading

mapping list_contents()
-----------------------

Return the mapping of contents

void set_title(string new_title)
--------------------------------

Set the title of the book.

string query_title()
--------------------

Return the title of the book.

void set_author(string who)
---------------------------

Set the author of the book

string query_author()
---------------------

Return the author of the book

int is_book()
-------------

Return whether or not the object is a book.