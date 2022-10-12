task_d
======

nomask string query_title(string task_id)
-----------------------------------------

 Return the title for a given task.

nomask string query_description(string task_id)
-----------------------------------------------

 Return the description for a given task.

nomask string query_owner(string task_id)
-----------------------------------------

 Return the owner of a given task.

nomask string query_creation_time(string task_id)
-------------------------------------------------

 Return the creation time of a given task.

nomask string query_status(string task_id)
------------------------------------------

 Return the status of a given task.

nomask mixed query_attribute(string task_id, string attr)
---------------------------------------------------------

 Return a particular attribute of the specified task.

nomask mixed query_attributes(string task_id)
---------------------------------------------

 Return all attributes of the specified task.

nomask mixed *query_sub_tasks(string task_id)
---------------------------------------------

 Return the sub-tasks for a given task.

nomask void set_title(string task_id, string title)
---------------------------------------------------

 Set the title of a given task.

nomask void set_description(string task_id, string desc)
--------------------------------------------------------

 Set the description of a given task.

nomask void set_owner(string task_id, string owner)
---------------------------------------------------

 Set the owner of a given task.

nomask void set_status(string task_id, string status)
-----------------------------------------------------

 Set the status of a given task.

nomask void set_attribute(string task_id, string attr, mixed val)
-----------------------------------------------------------------

 Set an attribute of a given task.

nomask void remove_attribute(string task_id, string attr)
---------------------------------------------------------

 Remove an attribute of a given task.

nomask void clear_attributes(string task_id)
--------------------------------------------

 Clear all attributes of a given task.

nomask private int check_completed(mixed *task_list)
----------------------------------------------------

 return 1 if they are all completed, 0 otherwise.

nomask mixed complete_task(string task_id)
------------------------------------------

 Only possible if all sub-tasks are also completed.

nomask private *find_task(string task_id)
-----------------------------------------

 and return the specified task, or 0 if error.

nomask string resolve_parent_id(string task_id)
-----------------------------------------------

 Returns "0" for a top-level task.

nomask mixed *query_task(string task_id)
----------------------------------------

 Return a copy of the specified task.

varargs nomask mixed *query_tasks(string task_id)
-------------------------------------------------

 Return a copy of the tasks array.

string add_task(string parent_id, string title, string description, string who)
-------------------------------------------------------------------------------

 Returns the task id of the new task.

mixed *remove_task(string task_id)
----------------------------------

 Remove the specified task.