====================
Skill system in LIMA
====================

This is a guide to the skill system in LIMA. Skills are quite flexible and can be adapted
to fit pretty much any purpose you have for your MUD.

A few facts:
   - Skills are part of a skill tree.
   - Skills train when they are used.
   - Both skill leaves and skill branches can be trained.
   - The value of a skill is equal to the value of the tree from the root to the branch.

Skills are part of a skill tree
-------------------------------

Skills in LIMA are positioned in a skill tree, and got names like "combat/defense/disarm".

.. image:: ../images/skill_tree.png
  :width: 700
  :alt: Skill tree

  Skill tree example.

The skills go from zero to a maximum value defined by:

.. code-block:: c

   // Each skill will start at 0, and go to this number of points.
   // Default: 10000
   // Range: 5000-30000
   // Type: integer
   #define MAX_SKILL_VALUE 10000

The comments above the ``#define`` are for easy maintenance via the `admtool <../command/admtool.html>`_.
The range here is a recommendation more than a hard limit. If you break the ranges, funny things might
start happening.

Skills train when they are used
-------------------------------
When the player uses a skill, it is trained. It will gain more if the skill check succeeds, and a little less
when it fails. Training will also be slower as you gain more point. This means players will have to keep
searching for new challenges, they can keep up the grind, but it will earn them less and less.

The main defines to fiddle with to set the speed of learning are:

.. code-block:: c

   // The max amount of points that can be gained for learning.
   // Default: 10
   // Range: 10-50
   // Type: integer
   #define TRAINING_FACTOR 10

   // As a person's skill increases, the amount they learn decreases. A higher value here will mean quicker ranks at first.
   // Default: 10
   // Range: 3-20
   // Type: integer
   #define LEARNING_FACTOR 10

When training a skill down the tree, there is a chance that some point will flow up the tree towards the root. 
So if you train a specific laser pistol, e.g., some of your training is valid for pistols in general.
Your skill value in a specific skill is the total of that branch, so the new pistol would have 0 in the 
specific skill, but would not be in total since you have experience with other pistols.

So, example:
   |  Combat 10
   |  Combat/pistol 10
   |  Combat/Laser Gun 100
   |  Combat/Laser Pew Pew 0

The total skill is an aggrgate of the the skill, so (a simple not completely correct example) 
"combat/pistol/Laser Gun" would be 10+10+100, where as "combat/pistol/laser pew pew" would be 10+10.

.. code-block:: c

   // The skill points learned move up the tree, divided by this number.
   // Default: 2
   // Range: 2-10
   // Type: integer
   #define PROPAGATION_FACTOR 2

The reason the example is not completely correct, is that the aggregated value is a factor of the
parents as defined as:

.. code-block:: c

   // A skill value is an aggregate of all the parents. 1/N^i of parent skills aggregate into the specified skill
   // Default: 3
   // Range: 2-5
   // Type: integer
   #define AGGREGATION_FACTOR 3

// Points learned by N on failure
// Default: 1
// Range: 1-5
// Type: integer
#define SKILL_ON_FAILURE 1

// Minimum to learn on a win
// Default: 2
// Range: 2-5
// Type: integer
#define SKILL_MIN_ON_WIN 2

// Maximum points to learn on a win
// Default: 20
// Range: 10-30
// Type: integer
#define SKILL_MAX_ON_WIN 20

// Do we use training points or not
// Default: yes
// Type: boolean
#define SKILL_CONFIG_USES_TRAINING_PTS

// Use roman numerals for skill ranks - no means plain numbers.
// Default: yes
// Type: boolean
#define USE_ROMAN_NUMERALS

#endif /* __SKILLS_H__ */
