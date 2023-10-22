# pv

> Monitor the progress of data through a pipe.
> More information: <https://manned.org/pv>.

- Print the contents of the file and display a progress bar:

`pv {{path/to/file}}`

- Measure the speed and amount of data flow between pipes (`--size` is optional):

`command1 | pv --size {{expected_amount_of_data_for_eta}} | command2`

- Filter a file, see both progress and amount of output data:

`pv -cN in {{big_text_file}} | grep {{pattern}} | pv -cN out > {{filtered_file}}`

- Attach to an already running process and see its file reading progress:

`pv -d {{PID}}`

- Read an erroneous file, skip errors as `dd conv=sync,noerror` would:

`pv -EE {{path/to/faulty_media}} > image.img`

- Stop reading after reading specified amount of data, rate limit to 1K/s:

`pv -L 1K --stop-at --size {{maximum_file_size_to_be_read}}`


# Custom ...........................................................................................
```bash
curl http://localhost:8000/stream | pv --line-mode --average-rate > /dev/null

# Creating a progress bar with the copy command
pv history.log > $HOME/Documents/history.log

pv history.log | zip>$HOME/Documents/history.zip

tar -czf - ./Documents/ | (pv -p --timer --rate --bytes > backup.tgz)
```
