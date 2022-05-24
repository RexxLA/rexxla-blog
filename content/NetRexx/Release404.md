---
title: "NetRexx 4.04"
date: 2022-05-23T15:41+02:00
tags: []
featured_image: ""
description: "NetRexx 4.04 Preview"
--- 
{{< tweet user="NetRexx" id="1529015744310157313" >}}

As always, you can build NetRexx 4.04 from the repositories at
[github](https://github.com/RexxLA/NetRexx) or
[sourceforge](https://sourceforge.net/projects/netrexx/), and that
only requires a working Java jre - or download the preview (at the
bottom of this page).

What you already get over NetRexx 4.03 is the following:
## A fix for a bug in Address
Under specific circumstances the address statement could generate a
duplicate internal variable when catching an
InterruptedException. This did not come out during the beta phase for
4.03, and has been fixed by Marc Remes.
## Rexx built-in-functions
For want of a better name, class `RexxRexx` offers the full set of
NetRexx built-in-methods in the classic function notation. Like
ooRexx, you can use this notation without including anything in simple
scripts like these:

```rexx
file: half.rexx
/* rexx */
parse arg number
if datatype(number,'whole')
then say number%2 + number //2
else say ' argument is not a whole number'
```
## Exit the NetRexx Workspace with ... 'exit'
Ever been in *plokta* mode? (Short for 'press a lot of keys to abort')
This happens in VI, Prolog, APL, etc, a lot, and we had this problem
until recent in nrws, the NetRexx Workspace. For the previously
mentioned tools you will have to remember 'wq!', 'halt.', and ')off' -
do you know what it is in nrws? (me neither).

Well, starting with release 4.04 you can just type *exit*.

Apart from this, there are a few other things:

- resolved an error in the startup timer of nrws; timer display is now default
- the default now is to display the command timer instead of the window number
- the default prompt changed to 'Ready;' (but both options can still be changed in an $HOME/nrws.properties file)
- the pipelines processor is now loaded by default - type in a
  pipeline like you do on VM/CMS, and it will be executed blindingly
  fast (just look at the timing display on the right)
## More to come
Please use the [issue
tracker](https://github.com/RexxLA/NetRexx/issues) to log issues and
requests.

## Download the preview
A regularly updated preview is downloadable from
[here](http://netrexx.org/files/NetRexxF.jar). This is a drop-in
replacement for the NetRexxF.jar file; if you rename the old one and
put this in its place, everything will work as before, only you will
have the 4.04 alpha preview.
