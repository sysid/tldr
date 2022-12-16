# sed

> Edit text in a scriptable manner.
> See also: `awk`, `ed`.
> More information: <https://www.gnu.org/software/sed/manual/sed.html>.

- Replace all `apple` (basic regex) occurrences with `mango` (basic regex) in all input lines and print the result to `stdout`:

`{{command}} | sed 's/apple/mango/g'`

- Execute a specific script [f]ile and print the result to `stdout`:

`{{command}} | sed -f {{path/to/script.sed}}`

- Print just a first line to `stdout`:

`{{command}} | sed -n '1p'`

- Replace all `apple` (basic regex) occurrences with `mango` (basic regex) in all input lines and save modifications to a specific file:

`sed '/{{line_pattern}}/d' {{filename}}`

- Print the first 11 lines of a file:

`sed 11q {{filename}}`

- Apply multiple find-replace expressions to a file:

`sed -e 's/{{find}}/{{replace}}/' -e 's/{{find}}/{{replace}}/' {{filename}}`

- Replace separator `/` by any other character not used in the find or replace patterns, e.g. `#`:

`sed 's#{{find}}#{{replace}}#' {{filename}}`


# Custom ...........................................................................................
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
