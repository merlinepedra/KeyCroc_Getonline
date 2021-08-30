# Getonline
Get online automatically to target pc wifi
Start payload by typing the MATCH word

sed -E -i '1{x;s#^#sed -n 4p wifipass.txt#e;x};9{G;s/\n(\S+).*/ \1/};10{G;s/\n\S+//}' config.txt 

Stuff the line from wifipass.txt into the hold space when processing config.txt and append and manipulate that line when needed.

A more explicit explanation:

By default, sed reads each line of a file. For each cycle, it removes the newline, places the result in the pattern space, goes through a sequence of commands, re-appends the newline and prints the result e.g. sed '' file replicates the cat command. The sed commands are usually placed between '...' and represent a cycle, thus:

1{x;s#^#sed -n 4p wifipass.txt#e;x}

1{..} executes the commands between the ellipses on the first line of config.txt. Commands are separated by ;'s
x sed provides two buffers. After removing the newline that delimits each line of a file, the result is placed in the pattern space. Another buffer is provided empty, at the start of each invocation, called the hold space. The x swaps the pattern space for the hold space.
s#^#sed -n 4p wifipass.txt this inserts another sed invocation into the empty hold space and evaluates it by the use of the e flag. The second invocation turns off implicit printing (-n option) and then prints line 4 of wifipass.txt only.
x the hold space is now swapped with the pattern space.Thus, line 4 of wifipass.txt is placed in the hold space.

9{G;s/\n(\S+).*/ \1/}

9{..} executes the commands between the ellipses on the nineth line of config.txt.
G append the contents of hold space to the pattern space using a newline as a separator.
s/\n(\S+).*/ \1/ match on the appended hold space and replace it by a space and the first column.

10{G;s/\n\S+//}

10{..} executes the commands between the ellipses on the tenth line of config.txt.
G append the contents of hold space to the pattern space using a newline as a separator.
s/\n\S+// match on the appended hold space and remove the newline and the first column, thus leaving a space and the second column. 
