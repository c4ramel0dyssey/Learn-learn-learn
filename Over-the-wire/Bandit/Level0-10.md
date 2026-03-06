# Level 0-10

## Level 0 -> 1

> The password for the next level is stored in a file called readme located in the home directory.

Easy.  
Command:  
```
cat readme
```  
Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If  

## Level 1 -> 2

> The password for the next level is stored in a file called - located in the home directory

We can't use the same command line as above when it comes to files with special characters.  
Try inputting "cat -" and it will give us error. Use the location of the file instead.  

Command:  
```
cat ./-
```
Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx  

## Level 2 -> 3  

> The password for the next level is stored in a file called --spaces in this filename-- located in the home directory

This is for the case which the file's name has spaces in it. Same way as the above but this time we need ""  

Command:  
```
cat ./"--spaces in this filename--"
```

Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx  

## Level 3 -> 4  

> The password for the next level is stored in a hidden file in the inhere directory.

To see what files are there in the directory, usually we use the command "ls." But, the keyword here is a "hidden file."
Some files might not appear just with the command ```ls.``` Therefore,  

Command:  
```
cd inhere
ls -a
cat "...Hiding-From-You"
```
Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ  

## Level 4 -> 5  

> The password for the next level is stored in the only human-readable file in the inhere directory.

human readable file = printable characters. In order to know what type of file are we dealing with, use the command ```file.```  

Command:  
```
cd inhere,
ls -a
file ./* (to see all files' type at once)
```
You will find one file with the ASCII type among the data types. That is the file we're looking for. Use ```cat``` to print the contents of the file and there you go, our password for the next level.

Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw  

## Level 5 -> 6  

> The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties: human-readable, 1033 bytes in size, not executable

A little side note for this level regarding the ```find``` command:
- type f: a regular file  
       d: directory  
      l: symbolic link  
      c: character devices  
      b: block devices  
      p: named pipe (FIFO)  
      s: socket  

- size b: 512-byte blocks (default)  
      c: bytes  
      w: two-byte words  
      k: Kilobytes  
      M: Megabytes  
      G: Gigabytes

You are going to be presented with a lot of directories and a lot of files. There's no way you're checking the properties for each. It's gonna be a waste of time. So what you can do is specify the properties of the file you're searching for in one comamnd line.  
  
Command:  
```
find -type f -size 1033c
cat ./maybehere07/.file2
```
Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG  

## Level 6 -> 7  

> The password for the next level is stored somewhere on the server and has all of the following properties: owned by user bandit7, owned by group bandit6, 33 bytes in size

Ok so now similar types of question but in a different realm: user, group  

Command:  
```
find / -type f -user bandit7 -group bandit6 -size 33c
```
And then just use the ```cat``` command on the directory pointing to the file that matches the properties.  

Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj  

## Level 7 -> 8  

> The password for the next level is stored in the file data.txt next to the word millionth

In this question, we will be using the ```grep``` command. It is used to search for specific words, phrases, or patterns inside text files, and shows the matching lines on your screen. It is significantly useful when you need to quickly find certain keywords or phrases in logs or documents.  

Command:  
```
grep millionth data.txt
```
Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc  

## Level 8 -> 9  

> The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

There are two ways to complete this challenge. It's either you are using the command ```sort -u``` or ```sort``` followed by ```uniq```. The first command sorts the input and removes duplicates in a single step. It ensures that the output is sorted and contains only unique lines. Meanwhile, ```uniq``` only removes adjacent duplicates. Meaning, if the command is used without ```sort```, input is unsorted and duplicates that are not next to each other will remain.  

So command:  
```
sort data.txt | uniq -u
```
Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM  

## Level 9 -> 10  

> The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

"One of the few human-readabe strings." That's a hint pointing to the command ```strings``` to extract the few strings available in the file.  

Command:  
```
strings data.tx | grep ===
```
Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey  
