---
title: Restoring thousands of files from the Windows Recycling bin
author: carl_zhou
date: 2023-06-28 10:33:00 +0800
categories: [Programming]
tags: [windows, powershell]
pin: false
math: true
mermaid: true
image:
  path: /assets/powershell.png
  lqip: data:image/webp;base64,imgdatahere
  alt: Windows Powershell Command Console
---

# The Problem with OneDrive and Cryptomator

I had a drive encrypted by Cryptomator, which I synchronized with my OneDrive account. Because of the way Cryptomator names the encrypted files, OneDrive thinks the files are ransomware related files. Thus the files were deleted from my OneDrive storage, but fortunately they were only moved to the recycling bin on my local computer.
I am not sure why OneDrive developers thought it was a good idea to remove ransomware encrypted files from OneDrive storage, decreasing chances of recovery.

After some research, I discovered I am not the only one with this problem, and this remains an open issue:

<https://github.com/cryptomator/cryptomator/issues/1059>

I ended up with thousands of files in my recycling bin on my local computer, which I had a difficult time restoring using the GUI "restore files". This would often freeze my computer. It was obvious that the GUI was not designed to handle such a scenario.

I had to write a Powershell script to solve the problem.

# The solution

## Overview of the script



## The final script

# Conclusion
