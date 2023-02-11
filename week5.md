# CSE 15L Week 5 Lab Report: Researching Commands

By Benjamin Johnson

## Contents

### [Introduction](#introduction-1)

### [Exact Matches Only: `-x`](#exact-matches-only--x-1)

### [Case Insensitive Search: `-i`](#case-insensitive-search--i-1)

### [Counting Matches: `-c`](#counting-matches--c-1)

### [Seeing Context of Matches: `-A`, `-B`, and `-C`](#seeing-context-of-matches--a--b-and--c-1)

## Introduction

For the past two weeks in CSE 15L, we've been learning about commands for searching, editing, and interacting with files. A few important commands we learned are:

- `less`: Allows you to view a file in your terminal and use arrow keys to navigate the file
- `find`: Finds files in certain locations whose names match a certain pattern
- `grep`: Finds lines in a file, or multiple files, matching a search string or pattern

For this lab report, we were tasked with researching and writing about four options or usages of one of these three commands. I chose `grep`.

The basic use of grep is `grep [pattern] [file or files]`. For example, to use grep to find all lines with the word "test" in a file with relative path `./test/testtext.txt`, you can use `grep test ./test/testtext.txt`.

The examples I'll be providing for commands come from a repository we used in lab. The repository can be found at https://github.com/ucsd-cse15l-w23/docsearch. Within this repostiory, there are a lot of text files in the `written_2` directory, so that's where I'll be searching with grep.

## Exact Matches Only: `-x`

Normally, `grep` returns all lines that contain the search pattern. For example, if you search for "test" and there is a line with "test11", then that line would be included in the search results. However, with the `-x` option, you can make grep return only lines that exactly match the search pattern.

Here are some examples of using grep with and without this option on the `docsearch/written_2` directory:

1. Searching for a newline char and indent

Without `-x`:

Command:

```bash
grep "
      " written_2/travel_guides/berlitz1/WhereToMadrid.txt
```

Output: It prints every single line of the file because every line ends with a newline char and then indent:

```
        ...[all the other lines of the file]
        fled as a youngster already consumed with religious visions — is
        secondary to the location. From this rocky hill you look back on the
        entire panorama of medieval Ávila.



```

With `-x`:

Command:

```bash
grep -x "
      " written_2/travel_guides/berlitz1/WhereToMadrid.txt
```

Output:

```



```

Now it only prints the lines that **only** contain a newline and indent; that is, blank lines (it looks like nothing is printed because there are only blank lines). In this case, using `-x` is useful because it allows us to only see the blank lines and not be overwhelmed by the output of the whole file.

2. Searching for a newline char and indent

Without `-x`:

Command:

```bash
grep "        trail." written_2/travel_guides/berlitz1/WhereToMalaysia.txt
```

Output:

```
        trails for viewing the proboscis monkey; they like to bed down in a
        trails into the islands’ jungle interior.
        trail.
        trails through the protected forests, home to the western tarsier,
```

With `-x`:

Command:

```bash
grep -x "        trail." written_2/travel_guides/berlitz1/WhereToMalaysia.txt
        trail.
```

Output:

```
        trail.
```

In this example, the `-x` option makes grep only find the line that only contains the word "trail". Without `-x`, it finds all lines that contain "trail." followed by any or no other characters. This type of search could be useful if you want to see if any paragraphs end with a certain word like "trail": if you do an exact search with `-x`, then you only get lines that end with "trail.", but if you don't use `-x`, then you would also get lines that start with "trail." and then contain other sentences, which means they aren't at the end of a paragraph.

Source: https://phoenixnap.com/kb/grep-command-linux-unix-examples#:~:text=The%20grep%20command%20prints%20entire,%2C%20add%20the%20%2Dx%20option.&text=The%20output%20shows%20only%20the%20lines%20with%20the%20exact%20match.

## Case Insensitive Search: `-i`

Normally, `grep` only finds lines that exactly match the search pattern, including the text's case (upper vs. lowercase). So, for example, if a file contains the word "TEst" and you search for "Test" with grep, your search wouldn't find that line since the cases are different. However, the `-i` option allows you to do a case-insensitive search, meaning that it finds all matches with the same text as your search pattern, even if the case is different. So searching for "Test" with the `-i` flag would include the "TEst" line in its results, in our example.

Here are some examples of using this option to search `docsearch/written_2`:

1.

Command:

```bash
grep -i "where to go" written_2/travel_guides/berlitz1/WhereToMalaysia.txt
```

Output:

```bash
      Where to Go
```

In this example, the `grep` search located the "Where to Go" line, even though we searched for "where to go" in lowercase. The `-i` flag could be extremely useful for these searches if we knew that a file contained the words "where to go" but we didn't know how they were capitalized (since English title capitalization rules are so complicated and confusing). With the `-i` flag, we wouldn't have to think of the correct capitalization or repeatedly try different capitalizations, we could just search for any matches regardless of capitalization.

2.  Command:

```bash
 grep -i "hotels" written_2/travel_guides/berlitz1/HandRMadeira.txt
```

Output:

```
        Recommended Hotels
        Madeira’s hotels, many of them traditional, older style
        hotels have been urgently updating their design and services, as even
        more hotels are being built in the zona turista. At the same time, the
        visitors can now choose from hotels, quintas (villas), estalagens and
        most hotels charge a huge supplement) and for smaller hotels throughout
        VAT (value-added tax). All hotels accept major credit cards. For making
```

In this example, using `-i` with the search for "hotels" allowed us to find lines with "hotels" and "Hotels". For example, the first line in the output has "Hotels" (capital first letter), and later lines have lowercase "hotels". This type of search is useful when we want to see whenever a non-proper noun appears in a file, since that noun would only be capitalized when it's at the start of a sentence or in a title (as in "Recommended Hotels" in this example). With `-i`, we can find both those types of results.

Source: https://man7.org/linux/man-pages/man1/grep.1.html

## Counting Matches: `-c`

Grep usually prints all matches for a search, but what if there are a lot of matches and you just want to know how many there are? The `-c` flag is perfect for that; it makes grep print only the number of times the search appears in the file, instead of the appearances themselves.

Here are some examples on the `docsearch` repo:

1.  Command:

```bash
grep -c "Italy" written_2/travel_guides/berlitz1/WhereToItaly.txt
```

Output:

```
78
```

In this example, the command searches for "Italy" in the travel guide for Italy. As expected, there are a lot of matches, and using `-c` made grep print the total number of matches: 78. This way, we didn't have to look through the 78 matches and count up the total ourselves, of use another command like `wc` (word count); grep did all the work for us!

2.

Command:

```bash
grep "chapter" -i -c written_2/non-fiction/OUP/Abernathy/ch1.txt
```

Output:

```
11
```

In this example, I used grep with both the `-i` and `-c` flags to count the total number of times that the word "chapter" appears, case-insensitive, in Chapter 1 of Abernathy's non-fiction text. This command could be useful if I wanted to find which file out of several has the most occurrences of a word. In this case, if I wanted to find which chapter file had the most occurrences of "chapter", I could use the above command on each file and compare the results.

Source: https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/#:~:text=Grep%20is%20an%20essential%20Linux,system%20for%20developers%20and%20sysadmins.

## Seeing Context of Matches: `-A`, `-B`, and `-C`

Normally, `grep` only shows you the lines that match the search, but what if you want more context about those lines, such as the rest of the sentence a line is part of? For that, the `-A`, `-B`, and `-C` flags can be used. These flags must be followed by a number, such as `-A 2`, and they will print that number of lines before and/or after the matches along with the matches themselves. `-A` will print that many lines after, `-B` will print that many lines before, and `-C` will print that many lines before AND after.

Here are some examples of using these flags on the `docsearch` repository:

1.  Command:

```bash
grep -C 2 "Germany" written_2/travel_guides/berlitz1/IntroFrance.txt
```

Output:

```
        France has always been a haven for foreign artists. Not by
        accident did Van Gogh come from the Netherlands, Picasso from Spain,
        Max Ernst from Germany, and Chagall from Russia to make their home in
        France. One of France’s greatest poets of the 20th century, Wilhelm
        Kostrowitsky, better known as Guillaume Apollinaire, was born in Rome
```

In this example, I searched for the word "Germany" in the "Intro to France" travel guide, and there was one match. By using the `-C 2` flag, I made grep print the 2 lines before the matching line, then the matching line itself, then the 2 lines afterward. This is useful because it lets me see that the author is talking about Germany as an example of where foreign artists in France are from, and that the author is talking about artists and poets in this passage.

2.  Command:

```bash
grep -B 1 "money" written_2/travel_guides/berlitz1/IntroLosAngeles.txt
```

Output:

```
        outrageous, and can be seen in everything from clothing styles and
        choice of car to how Angelenos spend their time and money.
```

In this example, I used `-B 1` to find where the word "money" appears in Berlitz's Intro to Los Angeles travel guide, and to print the line immediately before the matching line as well as the matching line itself. This allows me to see that the author is talking about how Angelenos spend their money as an example of something outrageous, but to get more context on what that outrageous thing is, it would be useful to print more than one of the lines before, such as with `-B 2` or `-B 3`.

Source: https://www.geeksforgeeks.org/grep-command-in-unixlinux/
