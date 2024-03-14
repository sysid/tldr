# parallel

> Run commands on multiple CPU cores.
> More information: <https://www.gnu.org/software/parallel>.

- Gzip several files at once, using all cores:

`parallel gzip ::: {{path/to/file1 path/to/file2 ...}}`

- Read arguments from `stdin`, run 4 jobs at once:

`ls *.txt | parallel -j4 gzip`

- Convert JPEG images to PNG using replacement strings:

`parallel convert {} {.}.png ::: *.jpg`

- Parallel xargs, cram as many args as possible onto one command:

`{{args}} | parallel -X {{command}}`

- Break `stdin` into ~1M blocks, feed each block to `stdin` of new command:

`cat {{big_file.txt}} | parallel --pipe --block 1M {{command}}`

- Run on multiple machines via SSH:

`parallel -S {{machine1}},{{machine2}} {{command}} ::: {{arg1}} {{arg2}}`

<<<<<<< HEAD

# Custom ...........................................................................................
achieves parallel execution by dividing the input data into smaller chunks and then executing the specified command with each chunk in a separate process or on a separate machine.
This approach enables the concurrent processing of data and can lead to substantial time savings compared to executing the commands sequentially.

Here's an overview of how GNU Parallel achieves parallel execution:

1. Input splitting:
GNU Parallel splits the input data into smaller chunks, typically based on lines or records.
You can control the number of lines or records in each chunk, or let GNU Parallel decide the optimal size based on the number of available CPU cores.

2. Process management:
starts multiple processes to execute the specified command concurrently.
manages the number of running processes based on the available CPU cores and user-defined limits.
ensures that the system doesn't get overloaded by controlling the number of simultaneous processes.

3. Load balancing:
GNU Parallel balances the load among the available cores or machines to optimize resource usage.
It can even distribute jobs to remote machines over SSH if provided with a list of hosts.

4. Job execution:
Each chunk of input data is passed to a separate process running the specified command.
GNU Parallel takes care of starting the processes, passing the input data, and capturing the output.

5. Output collection: GNU Parallel collects the output from each process and combines it into a single output stream.
You can control the order in which the output is combined, either preserving the original input order or allowing outputs to be combined as they become available.

GNU Parallel achieves parallel execution by utilizing multiple cores or machines, splitting the input into manageable chunks, and efficiently managing the execution of jobs. This allows for a significant speedup in many situations where tasks can be processed independently and in parallel.
=======
- Download 4 files simultaneously from a text file containing links showing progress:

`parallel -j4 --bar --eta wget -q {} :::: {{path/to/links.txt}}`

- Print the jobs which `parallel` is running in `stderr`:

`parallel -t {{command}} ::: {{args}}`
>>>>>>> ec7be25438ea07b36254bfdfce5c1fa808a43e71
