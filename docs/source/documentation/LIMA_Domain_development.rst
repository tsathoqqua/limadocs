***********************
LIMA Domain Development
***********************

Prerequisites
-------------
A basic understand of LPC and how LPMUDs work is required. This can be gained from reading 
the `LPC Basics learning path <Basic_LIMA_Guide.html>`_. So at this point you need to understand
variables, function declarations, function calls, objects, flows, loops, some basic Linux commands
that will be useful for working with the MUD, and the filestructure used.

Help make this document better
------------------------------
This guide has been written for the newest FluffOS driver and LIMA Mudlib features.

If you spot errors and omissions, please submit an issue at https://github.com/tsathoqqua/limadocs/ and describe
the issue you found, or the addition you would like, or even better make a pull request.

Introduction
============
This document contains several parts, and it is recommended to read it at the order the chapters are
presented in, but they can be used as a reference at a later point should you need a refresher for 
a specific topic. The chapters will build up an ongoing example, and all the example code produced
here can be found under ``/domains/std/examples/`` in appropriate folders.

The ``/domains/std`` folder contains a lot of working examples for rooms, items and monsters that you
can refer to to learn more about features not documented directly in this guide.

CHAPTER 1: Creating a domain
============================
Domains are a mechanism that simplifies assigning (groups of) privileges to wizards and protections to directories.

Also, since the creation of domains and the inclusion of members in those domains is tightly controlled by 
`admtool <../command/admtool>`_, domains and their membership can be trusted in all contexts.  
This trust is used in certain parts of the mudlib to classify wizards and the commands and
options available to them. 

Each domain has a name, a set of "domain lords", and a set of members. Typically, it will also have a 
directory associated with it, such as ``/domains/name/``.  The name must be lower case, alphabetic characters.
The domain lords are the wizards who allow/revoke membership in the domain and have the control privilege 
for the domain. The domain members simply have domain data privileges.

.. note::
    
    An admin is just a wizard who is a member of the admin domain. You can make a wizard an admin
    by adding them to this domain using the admtool. Domain controls are under '1' (admin privilege),
    then "Domain".


