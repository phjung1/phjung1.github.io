---

layout: post
title: Vim Command
date: 2021-12-23 21:00:00
author: "phjung1"
header-img: "#"
tags:

- vim
- command

---

# Sumulation Vim on the Page



## Showing the Cursor Position in a Buffer



![](https://raw.githubusercontent.com/phjung1/imageUploader/main/2021/12/23-21-04-50-2021-12-23-21-04-46-image.png)



in row 2 we run the ***dw*** command to delete the word under the cursor. We can see how the buffer looks immediately after running this command by looking at the cotents of the buffer in the same row.





## Highlgting Search Mathches

![](https://raw.githubusercontent.com/phjung1/imageUploader/main/2021/12/23-21-07-17-2021-12-23-21-07-14-image.png)

When demonstrating Vim’s search command, it’s helpful to be able to highlight
any matches that occur in the buffer. In this example, searching for the string
“the” causes four occurrences of the pattern to be highlighted:



## Selecting Text in Visual Mode

![](https://raw.githubusercontent.com/phjung1/imageUploader/main/2021/12/23-21-08-29-2021-12-23-21-08-25-image.png)

Visual mode lets us select text in the buffer and then operate on the selection. 

here. We use the **it** text object to select the cotents of the **<a>** tag:




