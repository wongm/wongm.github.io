---
layout: post
title: Test data for date formatting unit tests
author: wongm
comments: true
tags: [unit testing, date formatting]
excerpt_separator: <!--end_excerpt-->
---

The other day I was looking over some unit tests for a date formatting code that handles both US and Australian locales, and noticed the test data being used - January 1. Not exactly a good choice, because even if your code is broken, the output looks the same as if it was working! 

<img src="/images/Corporate needs you to find the differences meme - They're the same picture - with two idential dates.jpg" alt="Corporate needs you to find the differences meme with two idential dates, text below reads They're the same picture." />

My solution was to change it to use January 31 instead - because there's no such thing as a 31st month! 😄