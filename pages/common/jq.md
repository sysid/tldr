# jq

- Output all elements from arrays (or all the values from objects) in a JSON file:
`jq '.[]' {{file.json}}`

- Read JSON objects from a file into an array, and output it (inverse of `jq .[]`):
```bash
seq 10 | jq -s '.
```

- Output the element-slice in a JSON file:

`jq '.[n:m]' {{file.json}}`

- Output the value of a given key of each element in a JSON text from stdin:
