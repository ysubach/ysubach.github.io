---
layout: post
title: "Treating files as sets: comm utility"
tags: [linux, unix, command line] 
---
Sometimes during administration work with real system you can have a
simple task: get list of strings and find subset of this list missing in
other list. Seems to be trivial but if you have thousands of values,
then it's time to use some automated solution.

Fortunately you don't need to write own program because `comm` utility
can do our task perfectly. It analyses 2 files (file1 and file2) as sets of strings and
allows to get:

* lines only in file1;
* lines only in file2; 
* and lines in both files.

Let us see how it works on a very simple example: _file1_ - list of
files archieved yesterday, _file2_ - list of current files. In order to
be correctly processed these files must be sorted, it's simple - run
`$ sort file1 > file1_s` and `$ sort file2 > file2_s`.

Now use `comm` to get following useful information:

* list of files deleted today (lines only in file1):
  
  ```
  $ comm -23 file1_s file2_s
  ```

* list of files added today (lines only in file2):

  ```
  $ comm -13 file1_s file2_s
  ```

* list of files present yesterday and today (lines in file1 and file2):

  ```
  $ comm -12 file1_s file2_s
  ```

Hope it'll be useful in some situations. For more details see `man
comm`.
