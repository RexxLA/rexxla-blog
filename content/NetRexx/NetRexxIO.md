---
title: "Ways to read a file in NetRexx"
date: 2022-05-20T18:36:46+02:00
tags: []
featured_image: ""
description: ""
---
NetRexx is fairly versatile in providing ways to do file I/O. This is because
different languages contributed their I/O paradigms. For the user,
this offers flexibility; but the fact that there is choice, does not necessarily
imply there is complexity; you can choose your favourite pattern and
use that. Some seem to fit specific activities better than other.

The purpose of this post is to show the possibilities you
have on the Windows, Linux and macOS operating systems. For now, we
will leave out the additional ways the z/OS mainframe offers. All code
examples show how to read all the lines in a file, and output them to
the terminal, prefixed with a line number.

## The Classic Rexx way
Starting with NetRexx 4.03 the Classic Rexx `stream` methods are
provided in the `netrexx.lang.RexxStream` class; in scripting mode,
these are automatically provided: we issue a 'uses RexxStream' so its
static methods can be used without their classname prefixed.
```
/* Rexx and NetRexx >= 4.03 */
parse arg filename
do i=1 while lines(filename) > 0
  say i linein(filename)
end
```
This is compatible between Classic Rexx and NetRexx for a number of
reasons. Due to scripting mode (where you do not define classes and
methods) the variable `arg` receives the commandline arguments; the
`parse` from NetRexx has a variable as first argument, so it will
break up the commandline arguments correctly. Furthermore, correct
NetRexx will require `loop` instead of `do`, but NetRexx tolerates
this and corrects it automatically. (Also, ooRexx and Regina know how
to use `loop`, so for those interpreters we might as well use that -
but we leave z/VM Rexx, RexxC370 and BREXX behind there).  


## Event based
In the below example, by Ruurd Idenburg, the `netrexx.lang.RexxIO` class is
used. This enables an event based way to implement IO. This has the
advantage that all the code that actually handles the lines, can be
bundled together in the `handle()` method, and separated from the code
that sets up the arguments and opens the file. The `handle()` method
can also be put into another class (which needs to implement the
`LineHandler` interface.
```
class ReadFileByLines public implements LineHandler

properties private static
fileName = String
lineNo = 0

  method main(args=String[]) public static
    lineHandler = ReadFileByLines()
    loop fileName over args
      lineNo = 0
      inputFile = RexxIO().File(fileName)
      if (inputFile<>null)
	then inputFile.forEachline(lineHandler)
      else say "File:" fileName "cannot be found!"
    end
    
  method handle(line)
    lineNo = lineNo+1
    say fileName':' lineNo "-" line
```
## Using address
Since NetRexx 4.03, the `address` statement has the 'with output stem'
option, which seems a good fit for this assignment. 

```
istem=''
address system 'cat testfile' with output stem istem
loop i=1 to istem[0] 
  say i istem[i]
end
```
Note that in NetRexx, stem notation uses square brackets
exclusively. For Windows, 'type testfile' instead of 'cat testfile'
can be used.
## Plumbing - Using a pipeline
NetRexx enables you to use pipelines, a lesser-know but magnificent
data stream oriented language that also originated on VM - IBM mainframe
VM that is.

Pipelines have sources, stages and sinks. 'Cons' (short for 'console')
is here the data sink, the '<' sign is the data source, which sends
the records of 'testfile' into the pipeline. The 'spec' stage does the
work: it prints the record number on position 1 and the rest of the
record (1_\*) to the 'nw' (next word) position. Pipelines is a
powerful tool with a steep learning curve, but I think worth your while. 
```
pipe < testfile | spec number 1 1-* nw | cons
```
When you start `nrws` in the directory that has
`testfile`, and type in this line, the output will be the file
prepended with the record number. On the command line, the part after
'pipe' needs to be quoted. To compile the pipeline to a Java class, it
needs to be in an '.njp' file and compiled with the `pipc' compiler -
all included with NetRexx.

## Employing the Java I/O classes
The original way to handle I/O in NetRexx was to use the Java
classes. This has the advantage of being able to use the vast array of
functionality in the Java class libraries (which also include binary
file I/O, random access to files by way of offset, combination with
stream compression and encryption, and a multitude of other features -
even if we leave out the modernisation the java.nio package offers).

It has the drawback of needing to learn the Java way of doing I/O, and
being somewhat verbose when compared to the more Rexx-like ways. For
that reason it is not my first choice for small scripting like
activities; for a large application it offers all the speed and
flexibility you need.
```
import java.io.BufferedReader
import java.io.FileReader

class ReadFileByLines public
  
  method processLine(lineNo=int,line=String) private static
    say lineNo || " - " || line
    
  method main(args=String[]) static
    loop filename over args
      do
	fr=FileReader(filename)
	br=BufferedReader(fr)
	lineNo=0
	loop forever
	  line=br.readLine()
	  if line==null then leave
	  processLine(lineNo,line)
	  lineNo=lineNo+1
	end
      catch ex=Exception
	ex.printStackTrace()
      finally
	if fr\==null then do
	  do
	    br.close()
	  catch ey=Exception
	    say ey
	  end
	do
	  fr.close()
	catch ez=Exception
	  say ez
	end
	end
      end
    end
```
This example is from Jason Martin on the NetRexx list. More examples are listed in Chapter 9, 'Handling Files' of [Creating Java Programs using NetRexx, IBM Redbook SG242216](http://www.netrexx.org/files/docs/sg242216.pdf).
