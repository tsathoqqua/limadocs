m_input
=======

.. c:function:: protected varargs nomask void input_one_arg(
    string arg_prompt,
    function fp,
    string arg
    )

Get a single argument and call a given function pointer with it.  If the
argument isn't present (arg == 0), then the prompt is used to ask for
the value.

.. c:function:: protected varargs nomask void input_two_args(
    string arg1_prompt,
    string arg2_prompt,
    function fp,
    string arg
    )

Get two one-word arguments and call a given function pointer with them.
Some or all of the arguments may be passed in initially; if they are not
all present, then the prompts are used to ask for the values.

.. c:function:: protected varargs nomask void input_three_args(
    string arg1_prompt,
    string arg2_prompt,
    string arg3_prompt,
    function fp,
    string arg )

Get three one-word arguments and call a given function pointer with them.
Some or all of the arguments may be passed in initially; if they are not
all present, then the prompts are used to ask for the values.m_input
=======

.. c:function:: protected varargs nomask void input_one_arg(
    string arg_prompt,
    function fp,
    string arg )

Get a single argument and call a given function pointer with it.  If the
argument isn't present (arg == 0), then the prompt is used to ask for
the value.

.. c:function:: protected varargs nomask void input_two_args(
    string arg1_prompt,
    string arg2_prompt,
    function fp,
    string arg )

Get two one-word arguments and call a given function pointer with them.
Some or all of the arguments may be passed in initially; if they are not
all present, then the prompts are used to ask for the values.

.. c:function:: protected varargs nomask void input_three_args(
    string arg1_prompt,
    string arg2_prompt,
    string arg3_prompt,
    function fp,
    string arg )

Get three one-word arguments and call a given function pointer with them.
Some or all of the arguments may be passed in initially; if they are not
all present, then the prompts are used to ask for the values.