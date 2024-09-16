# bandit0

## Key lessons

- ssh, cat

## How to

1. Login as bandit0 to bandit.labs.overthewire.org at port 2220:

```
    ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```

2. Check white files are at the home directory:

```
    ls
```

3. Read the contents of the 'readme' file:

```
    cat readme
```

# bandit1

## Key lessons

- cat files with special characters

## How to

1. login via ssh

2. Read the contents of '-' file:
```
    cat ./-
```

# bandit2

## Key lessons

- cat files with space characters

## How to

1. Read the contents of "spaces in this filename":
```
    cat spaces\ in\ this\ filename
```

# bandit3

## Key lessons

- ls hidden files

## How to

1. change directory to "inhere":
```
    cd inhere
```

2. List the hiddent contents of the directory:
```
    ls -als
```

3. Read the contents of "...Hiding-From-You":
```
    cat ...Hiding-From-You
```

# bandit4

## Key lessons

- determining the type of file using file command

## How to

1. navigate to "inhere":
```
    cd inhere
```

2. list the contents of the directory:
```
    ls
```

3. iterate through the files to determine the file type:
```
    for file in *; do
        file ./$file
    done
```

4. find the human readable file and read it:
```
    cat -file0#
```

# bandit5

## Key lessons

- using the find command

## How to

1. navigate to "inhere"
```
    cd inhere
```

2. find the file with 1033 bytes in size:
```
    find . -size 1033c
```

3. open the file

# bandit6

## Key lessons

- more with the find command and grepping

## How to

1. Go to the /:
```
    cd /
```

2. find the file with 33 bytes in size and grep for bandit7 and bandit6:
```
    find . -size 33c 2>/dev/null -exec bash -c "ls -als {} | grep bandit7 | grep bandit6" \;
```

3. open the file

# bandit7

## Key lessons

- more grepping

## How to

1. grep for the word millionth
```
    grep millionth data.txt
```

# bandit8

## Key lessons

- sort and uniq

## How to

1. sort data.txt and then find the unique line:
```
    sort data.txt | uniq -u
```

# bandit9

## Key lessons

- strings and regex

## How to

1. pull only the readable strings from data.txt and grep for lines that's preceded with '=':
```
    strings data.txt | grep -E "^=+"
```

# bandit10

## Key lessons

- base64

## How to

1. decode the base64 encoding:
```
    base64 -d data.txt
```

# bandit11

## Key lessons

- using tr to shift characters

## How to

1. shift using tr:
```
     cat data.txt | tr 'n-za-mN-ZA-M' 'a-zA-Z'
```

# bandit12

## Key lessons

- unzipping using tar, gzip, bzip2

## How to

1. make a temporary directory and copy the data.txt file to it cd to the directory.
```
    mktemp -d
    cp data.txt /tmp/tmp.xxxxxxxx
    cd /tmp/tmp.xxxxxxxx
```

2. cat to see that it is a hexdump file and revert it back and save it to a file:
```
    xxd -r data.txt > data1
```

3. now uncompress the file by first checking the type of file and using appropriate program:
```
    file data#
```
- if the file is compressed via gzip: 
```
    mv data# data#.gz 
    gzip -d data#.gz    #this will produce an decompressed file named data#
```
- if the file is compressed via bzip2: 
```
    mv data# data#.bz2
    bzip2 -d data#.bz2  #this will also produce an decompressed file named data#
```
- if the file is compressed via tar:
```
    tar -xvf data#      #this will print the name of decompressed file
```
