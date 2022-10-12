hooks
=====

void add_hook(string tag, function hook)
----------------------------------------



void remove_hook(string tag, function hook)
-------------------------------------------

see: add_hook

void hook_state(string tag, mixed hook, int state)
--------------------------------------------------

state 'state'

varargs mixed call_hooks(string tag, mixed func, mixed start,
			 mixed *args...)
---------------------------------------------------------------------------------

see: add_hook