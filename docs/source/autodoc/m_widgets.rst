m_widgets
=========

nomask int i_simplify()
-----------------------

 has simplify turned on.

nomask int i_emoji()
--------------------

 has emojis turned on.

string on_off_widget(int on)
----------------------------

 "On " or "Off" for simplified view.

string green_bar(int value, int max, int width)
-----------------------------------------------

 A bar that is green with white dots when spent

string critical_bar(int value, int max, int width)
--------------------------------------------------

 A bar that change colour the lower it gets

string slider_red_green(int value, int max, int width)
------------------------------------------------------

 value with a X. 0 is the middle and max is minus on red axis and + on green axis.

string slider_colours_sum(int value, mapping colours, int width)
----------------------------------------------------------------

 where each number is bigger and strings are ANSI colours.