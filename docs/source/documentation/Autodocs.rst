reStructured text formatting
============================
The LIMA mudlib supports reStructured text (RST) for documentation. Modules, commands and functions are documentation by using in file comments.

Module documentation:
 |  ``//: MODULE``
 |  ``// This is the description of this module.``
 |  ``// $$see: another_module``

Documentation for functions:
 |  ``//: FUNCTION funcname``
 |  ``//This is the doc for function funcname``

Documentation for commands:
 |  ``//: COMMAND``
 |  ``//This is the doc for the (wiz) command``

Docummentation for player commands:
 |  ``//: PLAYER_COMMAND``
 |  ``// This is the doc for the player command``
 |  ``// $$see: other_cmd``

Documentation for hooks:
 |  ``//: HOOK``
 |  ``// This documents a hook called with call_hooks``

Example code:
 |  ``//: EXAMPLE``
 |  ``// This is an example to illustrate some code.``

To do items that needs handling:
 |  ``//: TODO``
 |  ``// What we'd like to do with this in the future``

Another way of adding things we need to fix later:
 |  ``// ### Something has to be fixed``
 |  ``// ### This doesn't need to start at the left margin``

reStructured text tips
----------------------
Most things about RST is handled by the RST_D when generating the documentation stored under */help/autodoc/*, but a few things are worth noting:

Indented text needs a pipe to not get wrapped into a single line:

.. code-block:: c

  //: PLAYERCOMMAND
  // USAGE
  //
  //   |  ``help``
  //   |  ``help <topic>``
  //

References to other help pages will be handled if written correctly in the comments. The "$$ see:" directive will be converted into links to other commands:

.. code-block:: c

  //: PLAYERCOMMAND
  //$$ see: idea, typo, bug, question
  // USAGE
  //    ``feedback``

Use double reverse apostrophes (\`\`) for marking commands and tips for wizards, e.g. ``score -t neon``.

File names should be surrounded by asterisks to make them stand out in *italic*.

Testing your work
-----------------
You will not be fluent in RST immediately, so testing your documentation initially will speed up the process of getting the layout that you want.

- Update the files on the MUD
- Run ``@RST_D->scan_mudlib()`` and wait for it to finish. It will automatically kick off HELP_D when it's done to update statistics used by the ``docs`` command.
- Pick the written file from */help/autodoc/* and copy it to an online RST viewer and check if the layout is as you want it to be, e.g. https://rsted.info.ucl.ac.be/ but others work as well.
- Once happy with the RST, add an RST tag as per below.

An RST tag should be added to the end of the file as:

.. code-block:: c

  //
  // .. TAGS: RST

It must be have empty line above to be recognized as a comment in RST. The RST_D and HELP_D will pick up these tags so they can be observed using the ``docs`` command. Other tags can be added as well and will be collected and summarized by the HELP_D. 

     ``HELP_D->query_tags()`` 

for a mapping of tags and their according files.

.. disqus::
