---
layout: post
title: Classic Puzzles
date: 2021-12-26 14:12:00
author: "phjung1"
header-img: "#"
tags:

- THINK LIKE A PROGRAMMER
- STRATEGIES FOR PROBLEM SOLVING
- The Fox, the Goose, and the Corn


---

# The Fox, the Goose, and the Corn

The first classic problem we will discuss is a riddle about a farmer who needs
to cross a river. You have probably encountered it previously in one form or
another.

## PROBLEM: HOW TO CROSS THE RIVER?

A farmer with a fox, a goose, and a sack of corn needs to cross a river. The farmer
has a rowboat, but there is room for only the farmer and one of his three items. Unfortunately, both the fox and the goose are hungry. The fox cannot be left alone with the goose, or the fox will eat the goose. Likewise, the goose cannot be left alone with the sack of corn, or the goose will eat the corn. How does the farmer get everything across the river?



Think about how much easier the puzzle be to solve if the possibility of taking one of the items back to the near shore was made explicit



By enumerationg all the possible operations, we can solve many poblems by testing every combination of operations until we find one that works. More generally, by restaing a problem in more formal terms, we can often uncover solutions that would have otherwise eluded us,



Le's forget that we already know the solution and stating this particular pullze more formanlly. First, we'll list our constraints. THe key constraints here are:



1. The farmer can take only one item in the boat.

2. The fox and goose cannot be left alone on the same shore.

3. The goose and corn cannot be left alone on the same shore.



This problem is a good example of the importance of constraints. If we remove any of these constraints, the puzzle is easy. If we remove the first constraint, we can simply take all three items across in one trip. Even if we can take only two items in the boat, we can take the fox and corn across and then go back for the goose. If we remove the second constraint (but leave the other constraints in place), we just have to be careful, taking the goose across first, then the fox, and finally the corn. Therefore, if we forget or ignore any of the constraints, we will end up with a Kobayashi Maru.

Next,le's list the operations, THere are various ways of stating the operations for this puzzle. We could make a specific list of the actions we think we can take:



1. Operation : Carray the fox to the far side ofthe river

2. Operation: Carray the goose the far side of the river

3. Operation: Crray the corn to the side of the river



Remember, though, that the goal of formally restating the problem is to
gain insight for a solution. Unless we have already solved the problem and
discovered the “hidden” possible operation, taking the goose back to the
near side of the river, we’re not going to discover it in making our list of
actions. Instead, we should try to make operations generic, or parameterized.



1. Operation: Row the boat from one shore to the other.

2. Operation: If the boat is empty, load an item from the shore.

3. Operation: If the boat is not empty, unload the item to the shore.



By thinking about the problem in the most general terms, this second list of operations will allow us to solve the problem without the need for an “ah-hah!” moment regarding the trip back to the near shore with the goose. If we generate all possible sequences of moves, ending each sequence once it violates one of our constraints or reaches a configuration we’ve seen before, we will eventually hit upon the sequence of Figure 1-3 and solve the puzzle. The inherent difficulty of the puzzle will have been sidestepped through the formal restatement of constraints and operations.



## Lessons Learned

What can we learn from the fox, the goose, and the corn?



Restating the problem in a more formal manner is a great technique for gaining insight into a problem. Many programmers seek out other programmers to discuss a problem, not just because other programmers may have the answer but also because articulating the problem out loud often triggers new and useful thoughts. Restating a problem is like having that discussion with another programmer, except that you are playing both parts.



The broader lesson is that thinking about the problem may be as productive, or in some cases more productive, than thinking about the solution. In many cases, the correct approach to the solution is the solution
