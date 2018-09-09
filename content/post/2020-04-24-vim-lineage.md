---
title: Vim Lineage
author: Bobby Burden III
date: '2020-04-24'
categories:
  - editors
tags:
  - programming
  - editors
  - history
---

In the beginning there was Unix. At least, that's where I want to start exploring how `vim` came to be.

For many trades, knowing your tool so well that it's transparent between you and your work is vital. And to me, that includes knowing the history that led up to the tools at hand. Today, I mostly use Visual Studio Code as my editor (with a plugin to give me `vi/vim` keybindings), and `vim` is my go to for quick edits and most "sysadmin" type changes.

But our story for how `vim` came to be starts with Unix, and more precisely with `ed`, the standard editor.

# Who is Ed?

In the Unix userland, there are many tools that we all expect to just be there: `cat`, `cp`, `dd`, `ls`, and so on. GNU calls this set of tools the ["coreutils"](http://git.savannah.gnu.org/cgit/coreutils.git/tree/src). But when Ken Thompson was developing Unix, he saw that three things were needed for development in Unix: an assembler, editor, and shell. In 1969, he developed `ed` to fit the role of the editor in his equation.

`ed`, to a modern user, is archaic. It follows a line editor paradigm, operating on just a single line at a time. Consider this file called `hello.c`:

```c
#include <stdio.h>

int main()
{
    printf("Hello, World!");
    return 0;
}
```

Compiling and executing gives us:

```bash
$ cc hello.c
$ ./a.out
Hello, World!$
```

Oops! We forgot to include a newline (`\n`) in our `printf()`. Let's fix that with `ed`!

```bash
$ ed hello.c
78

```

Alright. So what are we looking at?

`ed` has opened the file, and read in `78` characters. Now it's just waiting on us to give it commands. The easiest is `q` (and `<ENTER>`) to close `ed` and get back to our shell. But let's finish editing this file to insert a newline during the `printf()`.

```bash
$ ed hello.c
78
P
*pn
7       }
*1,7pn
1       #include <stdio.h>
2
3       int main()
4       {
5           printf("Hello, World!");
6           return 0;
7       }
*5c
    printf("Hello, World!\n");
^C
?
*p
    printf("Hello, World!\n");
*w
80
*q
$ cc hello.c
$ ./a.out
Hello, World!
$
```

The first command I entered was `P`. This toggles the command prompt in `ed`. This isn't required, but adds a `*` at the beginning of the commands I entered to make it a bit easier to read for this demonstration.

`pn` prints the current line with the line number. Note that it shows we are on line 7 (`ed` starts you on the last line in the file).

`1,7pn` passes a _range_ of 1 through 7 to `p` and we have `n` there as well so that we get line numbers. Now we can see our entire file. We are concerned with line 5, so let's edit that.

`5c` tells `ed` that we want to change line 5. Crazy right?
This doesn't return any output, so the next line was my input. In this case, it was 4 spaces followed by an updated `printf()` with the `\n` included. Ctrl-c ends the input.

`p` prints out the current line. Since we just changed line 5, that's our current line. So we see our change.

`w` writes the _buffer_ to disk and returns the amount of characters written, which was 80 in this case.

`q` quits `ed` as previously mentioned. Then I'm back on the shell and compile and run my code to see the new output with our freshly minted `\n`.

This is just a small example of `ed`. If you'd like to dive down the rabbit hole, just make sure you have `ed` installed and run `info ed` to read the Info pages. But even based on this small example, you can see how `ed` is modal. There's a command mode (the default), and then an insert mode where I was able to change a line.

But before we move on, let's look at one more feature of `ed`:

```bash
$ ed hello.c
80
g/re/p
    return 0;
q
$
```

The command here is `g/re/p`. Which **G**lobally searched for the **r**egular **e**xpression "re", and **p**rinted the result. If we added an `n` at the end, we would have also seen the line number (6 in this case).

That global searching of a file with a Regular Expression birthed a new program, keeping inline with the unix philosophy of a tool doing one thing well, and that's _basically_ how we ended up with our regex searching tool being called `grep`. But that's probably a whole other blog post.

At this point you may be wondering why `ed` just works line-at-a-time. Remember, we're dealing with a program developed in 1969. At the time, a user would most likely be interfacing with the computer via a [teletype](https://en.wikipedia.org/wiki/Teleprinter). A multiline display method just wasn't feasible or useable in most scenarios. Moreover, an idea for Unix was that it would be able to run on less expensive systems, like a PDP-7.

By mid-1972, Unix had 10 installations, and development was happening with `ed`.

That leads us nicely into the next chapter in the saga. Another Bell Labs creation: `sed`.

# Yea, what he sed

By 1974, Lee E. McMahon working at Bell Labs in New Jersey had developed `sed`. What he called a "Non-interactive Text Editor". `sed` gets its name from being a _stream_ editor.

Let's take a quick sidebar to talk about what it means to be a _stream_ editor. To facilitate the recurring theme of doing just one job and doing it well, we have [pipes](https://en.wikipedia.org/wiki/Pipeline_%28Unix%29) that allow you to pass the output of one command to the input of the next. For example, if we want to display the list of files in our directory we would use `ls`. If we want to count the lines in a file we use `wc` with the `-l` flag. We can combine those as shown below:

```bash
$ ls
a.out  hello.c
$ ls | wc -l
2
$
```

That's a really simple example, obviously, but it shows that using the pipe (`|`) we can pass the output of one command as the input to another.

Now imagine we want to change all of the indentation in our `hello.c` from four spaces to tabs instead. That's where `sed` comes in. We could do this with `ed`, but with `sed` we don't have to worry about moving around the file or even loading the entire file into memory first.

```bash
$ sed 's/    /\t/' hello.c
#include <stdio.h>

int main()
{
        printf("Hello, World!\n");
        return 0;
}
$
```

Here, our regular expression is finding four spaces and replacing that with a `\t` character in `hello.c`. Note that this doesn't write to disk. Instead, it just outputs the results of the changes. We can add ` > hello.c` to the end of our command to _redirect_ the output back to the `hello.c` file and replace the contents completely.

And again, this argument to `sed` is working line-at-a-time through the input to arrive at our results. But let's look at another example that takes advantage of `sed` being able to work on streams of data passed in via pipes.

```bash
$ date
Thu 23 Apr 2020 09:46:13 AM EDT
$ date | sed 's/\([a-z]\)/\U\1/g'
THU 23 APR 2020 09:46:25 AM EDT
```

In this example, we are taking the output of `date` and editing with `sed`. We are using a regular expression that matches all lowercase letters and converting them to their uppercase equivelant. The `g` at the end says to do it _globally_. Without that `g` switch, it would stop executing the regex after the first match. This shows that we don't have to write the output of `date` to disk first to manipulate it.

# The EXtended Editor and the original eVIl editor

By 1976, Bill Joy had began writing a "new" line editor for Unix called `ex`. This time for the first BSD release, 1BSD. Of course `ex` didn't live in a vacuum. `em` was another editor developed in the early 70s that was meant to utilize video terminals. To me, this influence seems to be what sparked the ideas that lead up to the eventual creation `vi`.
`vi` and `ex` are closely tied together. On my machine, the `vi` command is a symlink to `ex` for example.

```bash
$ ls -l `which ex`
-rwxr-xr-t 1 root root 247768 Nov 13 13:09 /usr/bin/ex
$ ls -l `which vi`
lrwxrwxrwx 1 root root 2 Nov 13 13:09 /usr/bin/vi -> ex
```

`vi` and `ex` exists as two sides of the same coin. `vi` is the _visual_ mode, and `ex` is the _line_ mode of the same editor. By 1979, in the Second BSD release, a user could (like today) type `vi` in their shell to go straight into the visual mode of `ex`.

If you've ever been curious about the usage of the `<ESC>` and `h j k l` keys in `vi`, look no further than the keyboard for the [ADM-3A](https://en.wikipedia.org/wiki/ADM-3A) terminal that Bill Joy used at the time. You'll see that the `<ESC>` key sits where your `<TAB>` key most likely sits today, and the `h j k l` keys have arrows on them that correspond to the cardinal directions that they represent in `vi`.

# Improving on a Classic

In 1987, Tim Thompson was using an Amiga ST. He developed a clone of `vi` called "Stevie" which was an abbreviation of "ST Editor for VI Enthusiasts". Bram Moolenaar, based on this work, began developing `vim` for the Amiga in 1988 with an initial public release in 1991.

The name `vim` was originally chosen to represent `Vi IMitation` but eventually came to mean `Vi IMproved` as it was ported to other systems. Among these improvements were new modes, extended regular expressions, a scripting interface for developing plugins, editing of archives (gzip, tar, etc), spell checking, splitting and tabbed windows, syntax highlighting, and a long list of other changes and additions. `vim` is described as "very much compatible with `vi`", but is not strictly compatible with the POSIX specification of `vi`.

As you can see, the evolution of these tools has a long-lasting legacy. Each iteration brought new ideas, but stood on the shoulders of the giants that came before them.

Today, `vim` is still in active development, with the latest release at time of writing being Vim 8.2, released in December of 2019. With projects like [Neovim](https://neovim.io/) being actively developed, the lineage of `ed` is alive and well, and the next 50 years of `ed` or `vi` (or however you see it) will continue to see improvements and new users.

# Conclusion

This was a _very_ brief history of Vim's lineage, and doesn't touch on the influence on (and of) POSIX, Emacs, or any of the other editors and programmers in the story. I hope you've learned a bit about your tools. Now get out there and use 'em!
