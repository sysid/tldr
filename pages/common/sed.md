# sed

> Edit text in a scriptable manner.
> More information: <https://www.gnu.org/software/sed/manual/sed.html>.

- Replace the first occurrence of a regular expression in each line of a file, and print the result:

`sed 's/{{regular_expression}}/{{replace}}/' {{filename}}`

- Replace all occurrences of an extended regular expression in a file, and print the result:

`sed -r 's/{{regular_expression}}/{{replace}}/g' {{filename}}`

- Replace all occurrences of a string in a file, overwriting the file (i.e. in-place):

`sed -i 's/{{find}}/{{replace}}/g' {{filename}}`

- Replace only on lines matching the line pattern:

`sed '/{{line_pattern}}/s/{{find}}/{{replace}}/' {{filename}}`

- Delete lines matching the line pattern:

`sed '/{{line_pattern}}/d' {{filename}}`

- Print the first 11 lines of a file:

`sed 11q {{filename}}`

- Apply multiple find-replace expressions to a file:

`sed -e 's/{{find}}/{{replace}}/' -e 's/{{find}}/{{replace}}/' {{filename}}`

- Replace separator `/` by any other character not used in the find or replace patterns, e.g. `#`:

`sed 's#{{find}}#{{replace}}#' {{filename}}`


```sh
# extract 10:26 from line
echo "US/Central - 10:26 PM (CST)" | sed -n "s/^.*-\s*\(\S*\).*$/\1/p"

-n      suppress printing
s       substitute
^.*     anything at the beginning
-       up until the dash
\s*     any space characters (any whitespace character)
\(      start capture group
\S*     any non-space characters
\)      end capture group
.*$     anything at the end
\1      substitute 1st capture group for everything on line
p       print it

# create backup
sed -i.bkp "/export RUN_ID/c\export RUN_ID=$(date +%Y%m%d)" "$PROJ_DIR/service/simulation/.env.local"

# replace line starting with
sed -i "/export GH_ENTERPRISE_TOKEN/c\export GH_ENTERPRISE_TOKEN=ghp_xxx" .envrc
```
