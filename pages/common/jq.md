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
```bash
  cat {{file.json}} | jq 'map(.{{key_name}})'

  ."key_with_special_character"

  # select the 9
  echo '{"k1": [{"k2": [9]}]}' | jq '.k1 | .[0] | .k2 | .[0]
```

- Output the value of multiple keys as a new JSON object (assuming the input JSON has the keys `key_name` and `other_key_name`):

`cat {{file.json}} | jq '{{{my_new_key}}: .{{key_name}}, {{my_other_key}}: .{{other_key_name}}}'`

- Combine multiple filters:

`cat {{file.json}} | jq 'unique | sort | reverse'`

- Output the value of a given key to a string (and disable JSON output):

`cat {{file.json}} | jq --raw-output '"some text: \(.{{key_name}})"'`

- raw strings
`echo '["1","2","3"]' | jq -r '.[]'`

- join list elements
`echo '["1","2","3"]' | jq -j '.[]'`

- create valid array instead of newline delimited lines: array constructor
`curl https://api.github.com/repos/stedolan/jq/issues?per_page=5 | jq '[ .[].title ]'`

- selecting multiple fields and generating valid json object: object constructor
```bash
curl https://api.github.com/repos/stedolan/jq/issues?per_page=2 | jq '[ .[] | { title: .title, number: .number} ]'
{(.timestamp): .value}  # evaluate key
```

- built-in functions: `sort, reverse, length, tostring`
`echo '["3","2","1"]' | jq 'sort'`


General object creation:
`jq '{ "key1": <<jq filter>>, "key2": <<jq filter>> }'`
```bash
# Everything is a fliter which can be combined via pipes (per value!):
curl https://api.github.com/repos/stedolan/jq/issues/2289 | \
  jq ' { title: .title, number: .number, labels: .labels | sort } '
  {
    "title": "Bump jinja2 from 2.10 to 2.11.3 in /docs",
    "number": 2289,
    "labels": [
      "dependencies",
      "feature request"
    ]
  }

# explode into key/values:
echo '{"k1": "v1", "k2": "v2"}' | jq 'to_entries'
# combine from key/values:
echo '[{"key":"k1","value":"v1"},{"key":"k2","value":"v2"}]' | jq 'from_entries'
```


`map(...)` let’s you unwrap an array, apply a filter and then rewrap the results back into an array.
shorthand for `[ .[] | ... ]`
`jq '[ .[] | { title: .title, number: .number, labels: .labels | length } ]'`
`jq 'map({ title: .title, number: .number, labels: .labels | length })`


## SELECT (equivalent to SQL where clause)
```bash
echo '[{"k1": "v1"}, {"k1": "v2"}]' | jq 'map(select(.k1 == "v1"))

curl https://api.github.com/repos/stedolan/jq/issues?per_page=100 | \
   jq 'map({ title: .title, number: .number, labels: .labels | length }) |
   map(select(.labels > 0))'

# filtering by key name rather than value
echo '{"user_id":123,"user_name":"duchess","order_id":456,"order_status":"sent","vendor_id":789,"vendor_name":"Abe Books"} | jq 'with_entries(select(.key|match("order_")))
```
## VARIABLES
```bash
echo '[1,2,3,4,5]' | jq '.|add as $x | length as $y | $x/$y
```
## STRING
```bash
# string interpolation http://stedolan.github.io/jq/manual/#Stringinterpolation-%5C%28foo%29
jq '.users[] | "\(.first) \(.last)"'
```

## REGEXP
```bash
cat $HOME/dev/binx/jq/car.log.json | jq '.|select(.name|test("b.*0"))|select(.msg == "running")'
```


# RECIPIES..........................................................................................

## Parsing JSON logs (k8s)
```bash
# grep: sanitize non json entries
# -M monochrome
# .["@timestamp"] special character in field name
# string interploation
k logs simulation-66fc459fcb-lbnp5 | grep '^{' | jq -M '"\(.["@timestamp"]) \(.level) \(.funcName)\(.lineno): \(.message)"
```

## Removing objects
```bash
# recursive descent/delete object
echo '[[{"a":1}]]' | jq '..|.a?'

# recursive delete https://stedolan.github.io/jq/manual/v1.6/
echo '[1,2, {"hello": {"delete_me": true, "a":3 }, "there": 4} ]' | jq 'walk(if (type == "object" and .delete_me) then del(.) else . end )'

# remove null values
jq '.["2021-01-03T16:59:00+00:00"] | select(. != null)'
jq 'walk( if type == "object" then with_entries(select(.value != null)) else . end)'
```

## Extracting objects recursively
```bash
curl 'https://hn.algolia.com/api/v1/items/27941108' \
  | jq '[recurse(.children[]) | del(.children)]' \
  | sqlite-utils insert hn.db items - --pk id
```
The jq recipe here is: `[recurse(.children[]) | del(.children)]`

The first `recurse(.children[])` recurses through a list of everything in a `.children` array.
The `| del(.children)` then deletes that array from the returned objects.
Wrapping it all in `[ ]` ensures the overall result will be an array.


## Split into files
[split into files per object](https://stackoverflow.com/questions/28744361/split-a-json-file-into-separate-files)
```bash
for f in $(cat x.json | jq -r '{(.timestamp): .charging_plans}|keys[]'); do
    cmd="cat xx.json | jq '.[\"$f\"]' > $f.xxx"
    echo $cmd
    "$FORCE" && eval "$cmd"
done
```

## MERGE
```bash
# merging two objects
jq -s '.[0] * .[1]' $HOME/dev/binx/jq/merge.json
```

## Filter for date
```bash
# filter for date with fractional seconds and create object
tail -10000 production_json.log | jq '. | select (.time | sub(".[0-9]+Z$"; "Z") | fromdateiso8601? > 1615550400) | {'method': .method, 'path': .path, 'id': .remote_ip, 'user': .username, 'time': .time, 'action': .action}'

# suspicios endpoint
tail -10000 api_json.log | jq '. | select (.time | sub(".[0-9]+Z$"; "Z") | fromdateiso8601? > 1615550400) | select (.path |contains("variable"))' | more

# fractional seconds and timezone offsets: An improved version that supports all offsets would be:
sub("(?<time>T[0-9:]+)(\\.\\d+)?(?<tz>Z|[+\\-]\\d{2}:?(\\d\\d)?)$"; .time + .tz) | strptime("%Y-%m-%dT%H:%M:%S%z")
```

## Other
```bash
# get all keys and count occurence
# 1. slurp
# 2. select entries which have key 'name
# 3. build new object
# 4. group object
# 5. foreach entry in grouped array
# 6. build result object:
# 6a: first entry: select unique/first entry, foreach select id
# 6b: second entry: count number of (sub)entries of grouped array
cat $HOME/dev/binx/jq/car.log.json | grep '^{' | jq -s '[.[]|select(has("name")) | {"name":.name}] | group_by(.name) | .[] | {"name": (unique| .[] | .name), "l": length} | select(.l > 2)'
```



# Gotcha............................................................................................
- fields with special characters: `.["@id"]`
- The shell is stripping quotes. It's good practice to single-quote the whole thing: `jq '.["@id"]' test.json`
- `jq-linux64` for latest features


# Tools.............................................................................................
- [interactive online jq](https://jqterm.com/?query=.)
- [gron](https://github.com/tomnomnom/gron) for quick find the path to a value in a deeply nested JSON blob
