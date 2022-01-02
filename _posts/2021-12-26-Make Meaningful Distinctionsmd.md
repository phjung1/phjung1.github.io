---

layout: post
title: Make Meaningful Distinctions
date: 2021-12-26 15:06:00
author: "phjung1"
header-img: "#"
tags:

- clen code

---

# Make Meaningful Distinctions

Programmers create problems for themselves when they write code solely to satisfy a compiler or interpreter. For example, because you can’t use the same name to refer to two different things in the same scope, you might be tempted to change one name in an arbitrary way. Sometimes this is done by misspelling one, leading to the surprising situation where correcting spelling errors leads to an inability to compile.



It is not sufficient to add number series or noise words, even though the compiler is satisfied. If names must be different, then they should also mean something different.



Number-series naming (a1, a2, .. aN) is the opposite of intentional naming. Such names are not disinformative—they are noninformative; they provide no clue to the author’s intention. Consider:



```cpp
public static void copyChars(char a1[], char a2[]) {
 for (int i = 0; i < a1.length; i++) {
 a2[i] = a1[i];
 }
 }
```



This function reads much better when source and destination are used for the argument names.



Noise words are another meaningless distinction. Imagine that you have a Product class. If you have another called ProductInfo or ProductData, you have made the names different without making them mean anything different. Info and Data are indistinct noise words like a, an, and the



Note that there is nothing wrong with using prefix conventions like a and the so long as they make a meaningful distinction. For example you might use a for all local variables and the for all function arguments.3 The problem comes in when you decide to call a variable theZork because you already have another variable named zork.



Noise words are redundant. The word variable should never appear in a variable name. The word table should never appear in a table name. How is NameString better than Name? Would a Name ever be a floating point number? If so, it breaks an earlier rule about disinformation. Imagine finding one class named Customer and another named CustomerObject. What should you understand as the distinction? Which one will represent the best path to a customer’s payment history?



There is an application we know of where this is illustrated. we’ve changed the names
to protect the guilty, but here’s the exact form of the error:



```cpp
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
```

How are the programmers in this project supposed to know which of these functions to call?



In the absence of specific conventions, the variable moneyAmount is indistinguishable from money, customerInfo is indistinguishable from customer, accountData is indistinguishable from account, and theMessage is indistinguishable from message. Distinguish names in such a way that the reader knows what the differences offer.


















