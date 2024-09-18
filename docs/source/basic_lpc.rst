.. index:: pair: Basic LIMA Guide; LIMA Mudlib
   :name: lima-guide

################
Basic LIMA Guide
################

This guide is a shortened form of the original guide, with updated links to modern resources used in 2024. 
Some of the links in this guide may be outdated depending on at which time you are reading this, so use your
favourite search engine to find the location of the new URL or a similar tool that works in 2080 or whenever
you read this.

LPC is a very easy programming language to learn, and it has real value in that place most of us know as 
the real world. Much of the syntax used for LPC, can be used for other programming languages.

You need knowledge of basic Linux (or Linux) commands like: ls, cd, mkdir, mv, rm, etc.

* A nice course by Ubuntu (51 minutes): https://ubuntu.com/tutorials/command-line-for-beginners 

* A command reference: https://mally.stanford.edu/~sr/computing/basic-Linux.html

For the purpose of this guide, we will assume you will be using Visual Studio Code (or an editor with similar
functionality). Code can be downloaded at https://code.visualstudio.com/download.

If you are editing locally, you can use Code directly to edit your files, otherwise the editor also
provides the ability to edit remote files. Using an SFTP extension, e.g., can provide a way of editing remote 
files transparently, or just using the "Open a Remote window" functionality inside Code. Find the best way 
that suits your setup.

So, at this point, it is assumed you know how to edit and write a file, and some basic Linux commands. 

If you know C, you are handicapped in that LPC looks a lot like C, but it is not C.  Your preconceptions about
modular programming development will be a hinderence you will have to overcome.  If you have never heard of the 
C programming language, then you are only missing an understanding of the simple constructs of C like the flow 
of program execution and logical operators and such.  So a C guru has no real advantage over you, since
what they know from C which is applicable to LPC is easy to pick up. The stuff they know about C which makes 
them a guru is irrelevant to LPC.
 
The chapters of this manual are meant to be read in order.  Starting with the introduction, going sequentially 
through the chapter numbers as ordered in the contents file.  Each chapter begins with a paragraph or two 
explaining what you should have come to understand by that point in your studies.  After those introductory 
paragraphs, the chapter then begins to discuss its subject matter in nauseating detail.  At the end of the 
chapter is a briefly worded summary of what you should understand from that chapter if it has been successful. 
Following that may or may not be some sidenotes relevant to the subject at hand, but not necessary to its 
understanding.
 
If at any time you get to a chapter intro, and you have read the preceeding chapters thoroughly and you do not 
understand what it says you should understand by that point, please contact us on the LIMA MUD.
 
Some basic terms this manual uses:

* **Driver** This is the executable program which is the game (typically FluffOS, DGD or LDMUD - LIMA uses 
  FluffOS).  It accepts incoming connections, interprets LPC code defined by the mudlib, keeps MUD objects 
  in memory, makes periodic attempts to clean unused MUD objects from memory, makes periodic calls to objects, 
  and so on. This is the Unreal Engine of MUDs.
 
* **Mudlib** LIMA is a mudlib, that contains LPC code which defines the world in which you are in.  The driver of 
  itself is not a game. It is just a program which allows the creation of a multi-user environment.  
  In some sense, the driver is like an LPC compiler, and the mudlib is like a compiler's library 
  (a very loose analogy).  The mudlib defines basic objects which will likely be used over and over again by 
  people creating in the MUD world.  Examples of such objects are ``/std/base_room``, ``/std/body.c``, and so on.

* **Your mudlib** Your mudlib begins as a copy of a mudlib, in this case LIMA. From this point on, you
  can modify it as you want, add and subtract as you see fit. Modifying the files contained in the LIMA 
  library might break the library and introduce changes that are not compatible with the future LIMA development,
  but there are ways to stay compatible with LIMA and still introduce changes, more in this later.

* **Domain** Your game files live in domains folders under ``/domains/``. Each domain has a lead developer, and
  perhaps other developers to help develop that domain. You can think of a domain as an area or specific scope of 
  your MUD, like a forest area, a space station or a dungeon area. Anything that seems self-contained and should
  be worked on by a subset of the developers.

* **Object** A room, a weapon, a monster, a player, a bag, etc.  More importantly, every individual file with 
  a .c extension is an object.  Objects are used in different ways.  Objects like ``/std/living.c`` are 
  inherited by objects like ``/std/body.c`` (used for all players) and ``/std/adversary.c`` 
  (used for all things that can be fought). Others are cloned, which means a duplicate of that code is loaded 
  into memory, e.g. all pistols or longswords rely on the same code, but are independent objects in the game
  world. And still others are simply loaded into memory to be referenced by other objects, a typical example
  of these are called daemons, more on those later.

**Credits**

Originally written by Descartes of Borg (borg@hebron.connected.com) around 1993, **adapted** and edited for
the LIMA library by Tsath (2024) using parts of the existing LIMA documentation.
                       
The original LPC Guide may be available at https://www.cs.hmc.edu/~jhsu/wilderness/basics.html if you are lucky.

**Help make this document better**

This guide has been changed, corrected and updated for the newest FluffOS driver and LIMA Mudlib features.

If you spot errors and omissions, please submit an issue at https://github.com/tsathoqqua/limadocs/ and describe
the issue you found, or the addition you would like, or even better make a pull request.


.. toctree::
   :caption: Basic LIMA Guide
   :numbered:
   :maxdepth: 4
   :titlesonly:

   basic_lpc/lpc_program
   basic_lpc/lpc_datatypes
   basic_lpc/lpc_functions
   basic_lpc/lpc_inheritance
