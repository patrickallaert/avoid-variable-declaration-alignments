Do NOT align variables declaration
==================================

This project is an attempt to demonstrate why variable declaration alignments is, in general, a bad thing while coding:

    example.php:
    <?php
    
    $bar     = "baz";
    $foo     = "bar";
    $myFoo   = "myBar";
    $myStuff = "foo";

When code like this is modified over the time, lines needs some rearrangments.

* The first reason to avoid this alignment is that automatic search/replace refactoring becomes harder if not impossible, e.g. by changing the variable *$myStuff* to *$myPersonalStuff*, that wouldn't correct the alignment of the first three variables.:

    $ sed -i "s/myStuff/myPersonalStuff/g" example.php  
This can be seen as a minimal annoyance but things becomes much more annoying with the time: e.g.: when you need to take the advantage of the *blame* feature of your VCS:
* As an exercise, in which commit has been introduced the *$bar* variable in: https://github.com/patrickallaert/avoid-variable-declaration-alignments/blob/master/example.php ?  
To discover this the steps using the web frontend are the following:
    1. Begin to **Blame** the file: https://github.com/patrickallaert/avoid-variable-declaration-alignments/blame/master/example.php
    2. See that it looks like introduced in ff5d20b: https://github.com/patrickallaert/avoid-variable-declaration-alignments/commit/ff5d20b53bfb330fab6741fdb100a5657ed50b8d
    3. Following the link, one can see that it existed before, view the file as it existed @ff5d20b: https://github.com/patrickallaert/avoid-variable-declaration-alignments/blob/ff5d20b53bfb330fab6741fdb100a5657ed50b8d/example.php
    4. Then go to the **History** of that file: https://github.com/patrickallaert/avoid-variable-declaration-alignments/commits/master/example.php
    5. Pick the commit that came before the ff5d20b one: https://github.com/patrickallaert/avoid-variable-declaration-alignments/commit/2f96a59c3b2a97e0e82b6bfe6933b2c6d3a760dd#example.php
    6. View the file in that version: https://github.com/patrickallaert/avoid-variable-declaration-alignments/blob/2f96a59c3b2a97e0e82b6bfe6933b2c6d3a760dd/example.php
    7. To finally redo a **Blame** on this specific version: https://github.com/patrickallaert/avoid-variable-declaration-alignments/blame/2f96a59c3b2a97e0e82b6bfe6933b2c6d3a760dd/example.php
    8. There you see that it looks like introduced in 3ae2a3b9: https://github.com/patrickallaert/avoid-variable-declaration-alignments/commit/3ae2a3b979cc8c7562e856fb6283ba1069d457ab
    9. Again, one can discover that this is not the commit that introduced it, repeat all the steps from 3. to 8.
    10. Finally, you will discover it has been introduced in: https://github.com/patrickallaert/avoid-variable-declaration-alignments/commit/071932d847146e0b9b400e3e9f08a4fcc7d6b320#example.php

Hopefully, this is a bit easier using the git command line client:

    $ git blame example.php
    $ git show ff5d20b5
    $ git blame example.php ff5d20b5~
    $ git show 3ae2a3b9
    $ git blame example.php 3ae2a3b9~
    $ git show 071932d  
But still! Far **way** too much operations IMHO for a usual task.

* Last but not least, by increasing the number of lines changed in every commits, chance is high to increase the number of possible conflicts. For the same reason it will also make backporting (cherry-picking) of fixes on those lines in your version branches much more difficult since likely to conflict.

If you are already convinced by the necessity of using atomic commits in revision control tools, time has come to also forget that kind of alignment aesthetics as it would prevent you from achieving all the goals of atomic commits. See more info on: http://en.wikipedia.org/wiki/Atomic\_commit#Atomic\_Commit\_Convention
