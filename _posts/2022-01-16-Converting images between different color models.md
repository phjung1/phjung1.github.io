---

layout: post
title: Relational vs Non-Relational
date: 2022-01-16 06:08:00
author: "phjung1"
header-img: "#"
tags:

- opencv

---

# Converting images between different color modles



in between input and output, when we apply computer vision techniques to images, we will typically work with three kinds of color models: grayscal, blue-greenred(BGR), and huesturation-value(HSV). Let's go over these briefly:



- GrayScale is a model that reduces color information by translating it into shades of gray or brightness. This model is extremely useful for the intermediate processing of images in problems where brightness information alone is sufficient, such as face detection. Typically, each pixel in grayscale image is represented by a single 8-bit value, ranging from 0 for black to 255 for white.

- BGR is the blue-green-red color model, in which each pixel has triplet of values representing the blue,green, and red componets or channels of the pixel's color. Web developers and anyone who wroks with computer grahics, will be familiar with a similar defination of colors, execpt with the reverse channel order.

- The HSV model uses a different triplet of channels. Hue is the color's tone,and value represents its brightness.

By default. OpenCV uses the BGR color model(with 8 bits per channel) to represent any image that it loads from file or captures form a camera


