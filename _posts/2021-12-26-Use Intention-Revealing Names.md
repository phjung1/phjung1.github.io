---
layout: post
title: Use Intention-Revealing Names
date: 2021-12-26 09:55:00
author: "phjung1"
header-img: "#"
tags:

- clen code

---

# Use Intention-Revealing Names

```cpp
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;


public List<int[]> getThem() {
 List<int[]> list1 = new ArrayList<int[]>();
 for (int[] x : theList)
 if (x[0] == 4)
 list1.add(x);
 return list1;
 }
```

Problem:

- What kinds of things are in theList?

- What is the significance of the zeroth subscript of an item in theList?

- What is the signifcance of the value 4?

- How would I use the list being returned?

theList -> gameBoard,

list1 -> flaggedCells

4 -> flagged 

list1 -> flaggedCells;

```cpp
public List<int[]> getFlaggedCells() {
 List<int[]> flaggedCells = new ArrayList<int[]>();
 for (int[] cell : gameBoard)
 if (cell[STATUS_VALUE] == FLAGGED)
 flaggedCells.add(cell);
 return flaggedCells;
 }
```

more then better

cell[STATUS_VALUE] == FLAGGED -> cell.isFlagged()

```cpp
public List<Cell> getFlaggedCells() {
 List<Cell> flaggedCells = new ArrayList<Cell>();
 for (Cell cell : gameBoard)
 if (cell.isFlagged())
 flaggedCells.add(cell);
 return flaggedCells;
 }
```
