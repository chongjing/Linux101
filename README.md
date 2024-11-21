# Linux 101

This is a workshop on basics of Linux commands, specifically for those with no any experience on Linux, `bash` Shell or Computing Clusters.



[TOC]

-[0. Set-up](###0. Set-up)

[5.Program Management](###5.Program Management)

### 0. Set-up

You can use three ways for this workshop:

- University HPC: you need to apply, https://www.hpc.cam.ac.uk/rcs-application, use your CRSid and password for log-in;
- MobaXterm: use free version, https://mobaxterm.mobatek.net/download.html
- WSL2: install WSL2 in Windows 11, https://learn.microsoft.com/en-us/windows/wsl/install
- MacOS: nothings need to do, good to go - Open Terminal

### 1. Linux basics

#### 1.1 What is Linux?

from Wikipedia [Linux - Wikipedia](https://en.wikipedia.org/wiki/Linux)

[Linux101/image-20241105134813245.png at main · chongjing/Linux101](https://github.com/chongjing/Linux101/blob/main/image-20241105134813245.png)

#### 1.2 Why Linux?

from Microsoft Copilot:

> Why computing servers and clusters usually use Linux system?
>
> **Open Source**: Linux is open-source, which means that its source code is freely available for anyone to inspect, modify, and enhance. This allows for greater customization and flexibility.
>
> **Stability and Reliability**: Linux systems are known for their stability and reliability. They can run for years without requiring a reboot, which is crucial for servers that need to be operational 24/7.
>
> **Security**: Linux is considered more secure than other operating systems. The open-source nature allows for continuous auditing and quick patching of vulnerabilities. Additionally, its permission and user management systems are robust.
>
> **Performance**: Linux can be optimized for performance. It efficiently manages system resources and can handle a large number of simultaneous connections, making it ideal for high-performance computing environments.
>
> **Scalability**: Linux can scale from a single server to a vast cluster of servers. It's used in various scales of computing, from small embedded systems to supercomputers.
>
> **Cost**: Since Linux is open-source, it is free to use. This reduces the overall cost for businesses and research institutions compared to proprietary operating systems that require licenses.
>
> **Compatibility**: Linux supports a wide range of hardware, from the oldest to the latest, making it highly versatile. Many scientific and technical applications are designed to run on Linux.
>
> **Community Support**: There is a large and active community of developers and users who contribute to Linux's development and offer support. This community-driven approach leads to continuous improvements and innovations.
>
> **Automation and Scripting**: Linux has powerful command-line tools and scripting capabilities, which are essential for automating administrative tasks and managing large clusters efficiently.

#### 1.3 Basic Commands:

Let us start with some simple commands to get familiar with Linux environment: type the command and hit the ENTER key.

```
$ date
Tue Nov  5 14:42:30 GMT 2024
$
```

In this example, `date` is our command, the output from this command shows the computer system’s date and will end with giving you an empty command line again. In fact, `date` is a program. It collects data from the system and uses the standard output to display the result. The standard output is the screen. If you want save your output, you could redirect the output into a file.

```
$ date > date.txt
$ cat date.txt
$ date >> date.txt
$ cat date.txt
```

The character `>` redirects the output of the command date into a file which is named `date.txt`. If it does not exist, this file is created automatically. Otherwise, it will be overwritten. You could use `>>”`to append the output to an existing file. Of course, since the output is redirected, you do not see the result. 

- **`pwd`:** Prints the current working directory.
- **`ls`:** Lists the contents of a directory.
- **`mkdir`:** Creates a new directory.
- **`cd`:** Changes the current working directory.
- **`.`:** Represents the current directory.
- **`../`:** Represents the parent directory.
- **`~`:** Represents the home directory of the current user.
- **`cp`:** Copies files or directories.
- **`mv`:** Moves or renames files or directories.
- **`rm`:** Removes files or directories.

- **`echo`**: Prints text to the terminal window

- **`touch`**: Creates a file

  Now let's use an simple script, save bellow code to `mycode.sh`. 

  ```bash
  #!/bin/bash
  sample_name="Nematode1"
  echo "Start Processing ${sample_name}"
  echo "Step1: Starting Quality Control"
  date
  echo "Step1 Completed"
  touch step1.done
  ```

  Run: `./mycode.sh`, you will get error - no permission. To change permission: `chmod u+x mycode.sh`.

- **`>`:** Redirects standard output to a file. If the file already exists, it will be overwritten.

- **`>>`:** Redirects standard output to a file. If the file already exists, the output will be appended to the end of the file.

  ```bash
  ./mycode.sh
  ./mycode.sh > out
  ```

- **`#`**: Comment out of code, to make comments in your script.

- **`history`**: list your most recent commands.

- **`<Tab>`**: Autocomplete.

- **`clear`**: Clear your terminal window.

- <u>**`Ctrl+C`**: Kill running process.</u>

Most commands accept options. These options influence the behavior of the command (which is, as said, in fact a program). Options are often single letters prefixed with a dash (-, also called hyphen or minus) and set off by spaces or tabs. Here is an example for `ls`.

```
ls
ls -a: list all files including hidden file starting with '.'
ls -d: list directories - with ' */'
ls -R: list recursively directory tree
ls -l: list with long format - show permissions
ls -hl: list long format with readable file size
alias ll='ls -hl'
```



### 2. Files

Some notes:

> - Everything in Linux is a file.
> - No special characters in paths and filenames.
> - Organize well.
> - Save your commands, make comments.

#### 2.1 File Attributes

When you apply the command `ls -l`, you can see that any file or directory is connected to certain attributes. 

```
$ ls -l mycode.sh
-rw-r--r-- 1 cx264 cx264 93 Nov  5 15:31 mycode.sh
$ chmod u+x mycode.sh
$ ls -l mycode.sh
-rwxr--r-- 1 cx264 cx264 93 Nov  5 15:31 mycode.sh
```

The following list describes the individual file attributes in more detail:

- Type: The first character indicates whether the item is a directory (d), a normal file (-), a block-oriented device (b), a character-oriented device (c) or a link (l). Actually, a directory is a special type of file. The same holds for the special directories “.” and “..”

- Access Modes: The access modes or permissions specify three types of users who are either allowed to read (r), write (w), or execute (x) the file. The first block of 3 characters indicates your own rights, the next block of 3 characters the group’s rights, and the last block of 3 characters the right for any user. The dash character (-) means that the respective permission is not set.
- Number of Links: the number of links or references to this file or folder. By default, there are two links to directory and one link to files. Any subdirectory increases the number of directory links by one.

- Owner: The user who created the file or directory, can be changed by `chown`.
- Group: This item shows to which group the file or directory belongs. The group ownership
  can be changed with the command `chgrp`.
  Size: he size of the file or directory.
- Modification Date and Time: the date and system time when the file was last modified

To change file attributes:

```
$ chmod o+x mycode.sh
$ ls -l mycode.sh
-rwxr--r-x 1 cx264 cx264   93 Nov  5 15:31 mycode.sh
```

Parameters for `chmod`:

```
User		Type		 Rights
u - user	+ add		r - read
g - group	- delete	w - write
o - others			x - execute
a - all
```

There is another way to change attributes using number code:

```
User		Group		Others
r w x		r w x		r w x
4+2+1		4+2+1		4+2+1
```

```
$ chmod 744 mycode.sh
$ ls -l mycode.sh
-rwxr--r-- 1 cx264 cx264   93 Nov  5 15:31 mycode.sh
$ chmod 745 mycode.sh
$ ls -l mycode.sh
-rwxr--r-x 1 cx264 cx264   93 Nov  5 15:31 mycode.sh
```

#### 2.2 Commands on Files

- **`find`:** Searches for files and directories that match certain criteria.

- **`cat`:** Concatenates and displays files.

- **`less`:** view the contents of a file one screen at a time.

- **`head`:** displays the first few lines of a file; default first 10 lines.

- **`tail`:** displays the last few lines of a file; default first 10 lines.

- **`sort`:** sort the lines of a file or input in a specified order, typically alphabetically or numerically.

- **`wc -l`:** counts the number of lines in a file 

- **`cut`:** extract specific sections or columns from a line of text in a file. It's useful for manipulating text data, such as extracting fields from a CSV file.

- **`paste`:** to merge lines from multiple files side by side.

- **`grep`:** Searches for a pattern in a file or files.

- **`tar`:** Archives files and directories into a single file.

- **`zip`:** Compresses files and directories into a single file.

- **`wget`**: Download files.

  ```bash
  wget http://www.ricesuperpir.com/uploads/common/gene_annotation/NIP-T2T.gff3.gz
  gunzip -c NIP-T2T.gff3.gz | less
  ```

#### 2.3 egrep

`egrep` is a command-line utility for searching plain-text data sets for lines that match a regular expression. Here are some basic functions of `egrep` in Linux with example codes:

- **Search for a string in a file:** The basic syntax when searching for a single file looks like this: `egrep [options] pattern [FILE]`. For example, to search for all information for gene `AGIS_Os01g010010` in `NIP-T2T.gff3` file, you would use the following command: 

  ```bash
  egrep AGIS_Os01g010010 NIP-T2T.gff3
  ```

  You can use options, for example to know how many chromosomes/sequences in a fasta file:

  ```bash
  egrep '^>' sequences.fasta | wc -l
  ```

  

- **Search for a string in multiple files:** To search for a string in multiple files, you can use the following command: `egrep -r "string" /path/to/directory`. This will search all files in the specified directory and its subdirectories.

- **Search for a string in a directory:** To search for a string in all files in a directory, you can use the following command: `egrep -r "string" /path/to/directory/*`.

- **Search for a string in a case-insensitive manner:** To search for a string in a case-insensitive manner, you can use the following command: `egrep -i "string" file.txt`.

- **Count the number of occurrences of a string:** To count the number of occurrences of a string, you can use the following command: `egrep -c "string" file.txt`.

- **Print lines that do not match a pattern:** To print lines that do not match a pattern, you can use the following command: `egrep -v "pattern" file.txt`.

#### 2.4 vi

`vi` is a **<u>text editor</u>** that is included with most Linux systems. (Personally, I use `vi` a lot, other text editors: nano, etc). Here are some basic functions of `vi` in Linux with example codes:

- **Open a file:** To open a file in `vi`, you can use the following command: `vi filename`. For example, to open the file "file.txt", you would use the following command: `vi file.txt`.
- **Insert text:** To insert text in `vi`, you need to be in insert mode. You can enter insert mode by pressing the "i" key. Once you are in insert mode, you can type your text.
- **Save changes:** To save changes to a file in `vi`, you need to be in command mode. You can enter command mode by pressing the "Esc" key. Once you are in command mode, you can save changes to the file by typing "`:wq`" and pressing "Enter".
- **Quit `vi`:** To quit `vi`, you need to be in command mode. You can quit `vi` by typing ":q" and pressing "Enter". If you have unsaved changes, `vi` will not quit. To force `vi` to quit without saving changes, you can type "`:q!`" and press "Enter".
- **Delete text:** To delete text in `vi`, you need to be in command mode. You can delete a single character by typing "x". You can delete an entire line by typing "dd". Delete # number of lines from cursor: #dd.
- Quickly go to specific line: "`#L`" and pressing "Enter".
- Delete # number of lines from cursor: `#dd`.
- Go to the end of the line: "`$`" and pressing "Enter".
- Replace string: `:%s/foo/bar/gc`



To replace a string using `vi` in Linux, you can use the substitute command. Here is the general syntax for string substitution in `vi` editor:

```
:[range]s/{pattern}/{string}/[flags][count]
```

- `[range]`: Specifies the range of lines to search. If you omit this parameter, `vi` will search the entire file.
- `{pattern}`: Specifies the pattern to search for.
- `{string}`: Specifies the string to replace the pattern with.
- `[flags]`: Specifies optional flags that modify the behavior of the substitute command. For example, you can use the "g" flag to replace all occurrences of the pattern on each line.
- `[count]`: Specifies how many occurrences of the pattern to replace on each line. If you omit this parameter, `vi` will replace all occurrences.

Here is an example of how to replace all occurrences of "Nematode1" with "Nematode1" in `mycode.sh`:

```
:%s/Nematode1/Nematode1/g
```

This command replaces all occurrences of "foo" with "bar" in the entire file.



### 3. Bash Programming

#### 3.1 sed

The `sed` command in Linux is used to perform different operations on files such as text replacement, insertion, text search, and more. Here are some examples of how to use sed command in Linux:

- Replacing or substituting string: In our `mycode.sh` job, suppose we have completed the first sample, now to process next sample:
```bash
$ sed 's/Nematode2/Nematode3/' mycode.sh # output to terminal
$ cat mycode.sh
$ sed 's/Nematode2/Nematode3/' mycode.sh > mycode2.sh
$ sed -i 's/Nematode2/Nematode3/' mycode.sh
```
- Deleting lines from a particular file: SED command can also be used for deleting lines from a particular file. SED command is used for performing deletion operation without even opening the file. Here are some examples:
```bash
$ sed 'nd' filename.txt # To Delete a particular line say n in this example.
$ sed '5d' filename.txt # To delete the fifth line of filename.txt.
```
- Inserting lines in a file: SED command can also be used for inserting lines in a file. Here are some examples:
```bash
$ sed '2i This is a new line.' filename.txt # To insert a new line at line number 2.
$ sed '$i This is a new line.' filename.txt # To insert a new line at the end of the file.
```
- Printing lines from a particular file: SED command can also be used for printing lines from a particular file. Here are some examples:
```bash
$ sed -n 'p' filename.txt # To print all lines of filename.txt.
$ sed -n '5p' filename.txt # To print the fifth line of filename.txt.
```

#### 3.2 awk

AWK is a text-editing tool that was developed by Aho, Weinberger, and Kernighan in the late 1970s. AWK can be used to do calculations as well as to edit text. Things that cannot be done with Sed can be done with AWK. Things you cannot do with AWK you can do with Perl. Things you cannot do with Perl you should ask a computer scientist to do for you.

- Print the whole content of a file:
```bash
$ awk '{print}' NIP-T2T.gff3
```
- Print specific columns of a file:
```bash
$ awk '{print $1"\t"$3"\t"$4"\t"$5}' NIP-T2T.gff3 # To print the first, third, forth, and fifth columns.
```
- Print a specific line of a file. For example, to get all gene positions:
```bash
$ awk '$3 == "gene" {print $1"\t"$3"\t"$4"\t"$5}' NIP-T2T.gff3

$ awk 'NR==5' test-file.txt # To print the fifth line of test-file.txt.
```
- Search for a specific word or pattern in a piece of text given:
```bash
$ awk '/AGIS_Os01g010010/ {print $0}' NIP-T2T.gff3 # To find all occurrences of “AGIS_Os01g010010” and prints those lines.
```

#### 3.3 Regular Expression

Regular expressions, often abbreviated as "regex" or "regexp," are sequences of characters that form search patterns. These are used for string matching, searching, and text manipulation (from Microsoft Copilot). Regular expressions do not work on their own. They are used together with a command or programming language, such as `egrep`, `find`, `vi`, `sed`, `awk`, `perl`, and many more. Examples (from Computational Biology, Second Edition, Röbbe Wünschiers):

```bash
Search Pattern 		Target String
(DNA|RNA) 		Search for “DNA” or “RNA”.
(D|R)NA 		Search for “DNA” or “RNA”.
[tmr]RNA 		Search for “tRNA”, “mRNA”, or “rRNA”.
*omics 			Search for “Genomics”, or “Proteomics”, or any other “nomics”.
b.g 			Matches any character between “b” and “g”, like big, bug, bag...
[a-zA-Z] 		Matches any single lowercase or uppercase character.
[˜ a-zA-Z0-9] 	        Matches any single character which is not a number or letter.
ˆ[eE]nzyme 		Matches the words “Enzyme” or “enzyme” at the beginning of a line.
[eE]nzyme$ 		Matches the words “Enzyme” or “enzyme” at the end of a line.
. 			Matches any single character except the newline character.
[ ] 			Matches any character listed between the brackets. 
[ˆ] 			Matches any character except those listed between the brackets.
[0–9] 			The dash (–) indicates a range of consecutive characters. 
? 		        Matches the preceding character or list zero or one time.
* 		        Matches the preceding character or list zero or more times.
+ 		        Matches the preceding character or list one or more times.
{num} 			Matches the preceding character or list num times.
{num,} 			Matches the preceding character or list at least num times.
{min,max} 		Matches the preceding character or list at least min times, but not more than max times. 
ˆ 			Matches the start of a line.
$ 			Matches the end of a line.
\ < 			Matches the beginning of a word.
\ > 			Matches the end of a word.
\b 			Matches the beginning or the end of a word.
\B 			Matches any character not at the beginning or end of a word.
```

Let's take an example, suppose we want to get positions of all genes from `NIP-T2T.gff3`, following a `bed` format (here is a description of `bed` format [BED File Format](https://grch37.ensembl.org/info/website/upload/bed.html)):

```bash
awk '$3 == "gene" {print $1"\t"$4"\t"$5"\t"$9"\t"$6"\t"$7}' NIP-T2T.gff3 | sed 's/ID=.*.Name=//g' > NIP-T2T.gene.bed
```

#### 3.4 Loops

In Linux, you can use **for** and **while** loops to automate repetitive tasks. Here are some examples:

- **`for` loop:** The `for` loop is used to iterate over a list of items. Here is an example that prints the numbers 1 to 5:

```
for i in 1 2 3 4 5
do
    echo $i
done
```

```
for i in 1 2 3 4 5;  do echo $i; done
```

Case: suppose you have 20 sample to process, you want make directory and save processed results for each of them

```bash
for i in {01..20}; do echo processing sample_${i}; mkdir sample_${i} && cd sample_${i} && echo completed sample_${i} && cd ..; done
# do not forget `cd ..`
```



- **`while` loop:** The `while` loop is used to execute a block of code as long as a certain condition is true. Here is an example that prints the numbers 1 to 5:

```
i=1
while [ $i -le 5 ]
do
    echo $i
    i=$((i+1))
done
```

In the above examples, the `echo` command is used to print the value of the variable `i`.

- **`until` loop:** The `until` loop is used to execute a block of code as long as a certain condition is false. Here is an example that prints the numbers 1 to 5:

```
i=1
until [ $i -gt 5 ]
do
    echo $i
    i=$((i+1))
done
```



- **`while read` loop:** The `while read` loop is used to read input from a file or command and execute a block of code for each line of input. Here is an example that reads lines from a file named `file.txt`:

```
while read line
do
    echo $line
done < file.txt
```

```
while read line; do echo $line; done < file.txt
```

Case: suppose you have a list of gene of your interest, you need to get position information from a .bed file. You can do:

```bash
$ cat mygene.list
AGIS_Os01g004820
AGIS_Os01g005690
AGIS_Os01g016990
AGIS_Os01g022830
AGIS_Os02g018710

$ while read line; do awk -v gene=$line '$4 == gene {print $0}' NIP-T2T.gene.bed; done < mygene.list
```



- **`for (( ))` loop:** The `for (( ))` loop is used to iterate over a range of numbers. Here is an example that prints the numbers 1 to 5:

```
for (( i=1; i<=5; i++ ))
do
    echo $i
done
```

- **`case` statement:** The `case` statement is used to execute different blocks of code depending on the value of a variable. Here is an example that prints a message depending on the value of the variable `day`:

```
case $day in
    Monday)
        echo "Today is Monday"
        ;;
    Tuesday)
        echo "Today is Tuesday"
        ;;
    Wednesday)
        echo "Today is Wednesday"
        ;;
    *)
        echo "I don't know what day it is"
        ;;
esac
```

#### 3.5 Variables

Environment variables are dynamic values that affect the processes or programs running on your Linux system. They are used to store information that can be used by system processes and user applications. Here are some examples of how to use environment variables in Linux:

- To view all environment variables:
```bash
$ printenv
```
- To view the value of a specific environment variable:
```bash
$ echo $HOME
```
- To set an environment variable:
```bash
$ which cellranger
# cellranger not found
$ export PATH=/rds/project/rds-FTKWLWDeHys/programs/CellRanger/cellranger-7.2.0/:$PATH
$ which cellranger
# /rds/project/rds-FTKWLWDeHys/programs/CellRanger/cellranger-7.2.0//cellranger
```
- To unset an environment variable:
```bash
$ unset VARIABLE_NAME
```

#### 3.6. A Bash Command Game

```
#!/bin/bash

# Chongjing Xia, 20241106, cx264@cam.ac.uk
# The script generates a random number between 1 and 100 and prompts the user to guess the number.
# Based on the user's input, it informs whether the guess is correct, too low, or too high,
# and continues until the user guesses correctly.

# RANDOM is a built-in system variable that generates a random number between 0 and 32767.
# Using the modulus operator, we convert this to a random number between 1 and 100.
num=$((RANDOM % 100 + 1))

# Use read to prompt the user to guess the number.
# Use if to compare the user's guess with the generated number: -eq (equal), -ne (not equal),
# -gt (greater than), -ge (greater than or equal to), -lt (less than), -le (less than or equal to).

while :
do 
 read -p "The computer has generated a random number between 1 and 100. Take a guess: " guess
    if [ $guess -eq $num ]   
    then     
        echo "Congratulations, you guessed it right!!"     
        exit  
    elif [ $guess -gt $num ]  
    then       
        echo "Oops, that's too high."    
    else      
        echo "Oops, that's too low." 
    fi
done
```




### 5.Progam Management

- Pre-compiled binaries
- Source code
- Bioconda - be aware of the license
- Containers - Docker and Singularity



### 6.Notes

a, programming languages

> "There are a number of such languages that are popular in bioinformatics (and in biology in general). This includes the eponymous scripting language of the GNU `bash` shell itself, `Python` and `R`. Each of these have their own strengths and weaknesses. `Bash` is ubiquitous and powerful but has a cumbersome syntax and is only really convenient for short programs. `Python` is a general purpose language with a very friendly syntax, and is nearly as ubiquitous as `Bash`. However, its ecosystem for bioinformatics analyses is relatively limited. `R` is not as prevalent as the other two but is excellent for manipulating and analyzing large amounts of data. Furthermore, it is the language of choice for bioinformatics analysis due to the large number of packages and tools it supports in this regard—especially for ‘-omics’ analyses through the Bioconductor ecosystem."   --from Raghavan et al. 20022. https://doi.org/10.1093/bib/bbab563



### **<u>Congrats, you've got 80% Linux knowledge, what's next?</u>**

***- a textbook***: [Computational Biology: A Practical Introduction to BioData Processing and Analysis with Linux, MySQL, and R | SpringerLink | by Röbbe Wünschiers](https://link.springer.com/book/10.1007/978-3-642-34749-8)

***- ChatGPT***: [ChatGPT](https://chatgpt.com/)

***- Microsoft Copilot***: [Microsoft Copilot: Your AI companion](https://copilot.microsoft.com/)
