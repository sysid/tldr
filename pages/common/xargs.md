# xargs

> Execute a command with piped arguments coming from another command, a file, etc.
> The input is treated as a single block of text and split into separate pieces on spaces, tabs, newlines and end-of-file.
> More information: <https://pubs.opengroup.org/onlinepubs/9699919799/utilities/xargs.html>.

- Run a command using the input data as arguments:

`{{arguments_source}} | xargs {{command}}`

- Run multiple chained commands on the input data:

`{{arguments_source}} | xargs sh -c "{{command1}} && {{command2}} | {{command3}}"`

- Delete all files with a `.backup` extension (`-print0` uses a null character to split file names, and `-0` uses it as delimiter):

`find . -name {{'*.backup'}} -print0 | xargs -0 rm -v`

- Execute the command once for each input line, replacing any occurrences of the placeholder (here marked as `_`) with the input line:

`{{arguments_source}} | xargs -I _ {{command}} _ {{optional_extra_arguments}}`

- Parallel runs of up to `max-procs` processes at a time; the default is 1. If `max-procs` is 0, xargs will run as many processes as possible at a time:

`{{arguments_source}} | xargs -P {{max-procs}} {{command}}`


# Custom  ..........................................................................................
https://www.oilshell.org/blog/2021/08/xargs.html:

- It's an adapter between text streams and `argv` arrays
- pass it flags that specify how to split stdin. Then it generates arguments and invokes processes.
```bash
# Example:
$ echo 'alice bob' | xargs -n 1 -- echo hi
hi alice
hi bob
```
What's happening here?
1. xargs splits the input stream on whitespace, producing 5 arguments, alice and bob.
2. We passed -n 5, so xargs then passes each argument to a separate echo hi $ARG command. By default, it passes as many args to a command as possible, like echo hi alice bob.

3 Things to master:
1. The algorithm for splitting text into arguments (-d, -0). Discussed below.
2. How many arguments are passed to each process (-n). This determines the total number of processes started.
3. Whether processes are run in sequence or in parallel (-P).

Choose One of 3 Ways of Splitting stdin
1. `xargs` (the default): when you want "words" without spaces. For example, you can produce two args from the string 'alice bob'.
1. `xargs -d $'\n'`: When you want the args to be lines, as in the egrep example above. (Note that $'\n' is bash syntax for a newline character, and Oil uses this syntax too.)
1. `xargs -0`: When you want to handle untrusted data. Someone could put a newline in a filename, but this is safe with NUL-delimited tokens.

avoid the mini language of `-I {}` and just use shell by recursively invoking shell functions.
Use `-n` To Batch Args, not `-L`

1. Incremental Development: Figure out what to do on each item (what's a task?), then figure out what items to do it on (what tasks should I run?)
2. Easy Testing by using echo to preview tasks. This avoids running long batch jobs on the wrong input!
4. xargs lets you start as few processes as possible.
5. It also lets you start those processes in parallel. You can't do this with a for loop.
6. Fewer Languages to Remember. We use plain shell and a few flags to xargs.
7. Composition via Pipelines. The task list becomes a "noun" that other shell tools can operate on.
```bash
# Filter tasks by name
find ... | grep ... | xargs ...

# Limit the number of tasks.  I use this all the time
# for faster testing
find ... | head | xargs ...

# Believe it or not, I use this to randomize music
# and videos :)
find ... | shuf | xargs mplayer
```

- When you use `-exec` to do the work you run a separate instance of the called program for each element of input.
   So if find comes up with 10,000 results, you run exec 10,000 times.
- With xargs, you build up the input into bundles and run them through the command as few times as possible, which is often just once. When dealing with hundreds or thousands of elements this is a big win for xargs.
- `-print0` Tells find to print all results to std, each separated with the ASCII NUL character '\000
- `-0` Tells xargs that the input will be separated with the ASCII NUL character '\000
    The advantage is that all results will be handed over to xargs as a single string without newline separation. NUL charater separation is a way to escape files which also contain spaces in their filenames
```bash
# blank is not separator
find . -name "*.jpg" -print0 | xargs -0 ls

# execut with 1 parameter
find .[args] -print0 | xargs -0 -n1 [cmd]

# print command and executie
echo 'one two three' | xargs -t rm

# change delimiter (allow spaces in path)
xargs -d '\n' mplayer

# define args as {}
find . -name '.envrc' -print0 | xargs -0 -I '{}' echo cp '{}' $HOME/dev/'{}'
```


## Multiple Arguements
```bash
echo argument1 argument2 argument3 | xargs -l bash -c 'echo this is first:$0 second:$1 third:$2'
```
