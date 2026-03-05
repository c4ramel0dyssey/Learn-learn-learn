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



