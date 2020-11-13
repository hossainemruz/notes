# How To?

## Manipulate File

### Check if a file exist

```bash
if [ ! -f /tmp/foo.txt ]; then
    echo "File not found!"
fi
```

### Find all files with specific extension of a directory and its subdirectories

```bash
find . -type f -name "*.conf"
```

### Parse file with shell script

```bash
#!/bin/sh
set -e

cmd="/usr/local/bin/docker-entrypoint.sh"
configFile="/usr/config/memcached.conf"

# parseConfig parse the config file and convert it to argument to pass to memcached binary
parseConfig() {
    args=""
    while read -r line || [ -n "$line" ]; do
        case $line in
            -*) # for config format -c 500 or --conn-limit=500
                args="$(echo "${args}" "${line}")"
            ;;
            [a-z]*) # for config format conn-limit = 500
                trimmedLine="$(echo "${line}" | tr -d '[:space:]')" # trim all spaces from the line (i.e conn-limit=500)
                param="$(echo "--${trimmedLine}")"                  # append -- in front of trimmedLine (i.e --conn-limit=500)
                args="$(echo "${args}" "${param}")"
            ;;
            \#*) # line start with #
                # commented line, ignore it
            ;;
            *) # invalid format
                echo "\"$line\" is invalid configuration parameter format"
                echo "Use any of the following format\n-c 300\nor\n--conn-limit=300\nor\nconn-limit = 300"
                exit 1
            ;;
        esac
    done <"$configFile"
    cmd="$(echo "${cmd}" "${args}")"
}

# if configFile exist then parse it.
if [ -f "${configFile}" ]; then
    parseConfig
fi
# Now run docker-entrypoint.sh and send the parsed configs as arguments to it
$cmd
```



### Create lots of random files

```bash
seq -w 1 10 | xargs -n1 -I% sh -c 'dd if=/dev/urandom of=file.% bs=$(shuf -i1-10 -n1) count=1024'
```

## Manipulate String

### Check first character of a string

#### Using **wildcard**

```bash
str="/some/directory/file"
if [[ $str == /* ]]; then
  echo 1;
else
  echo 0;
fi
```

#### Using **substring expansion**

```bash
if [[ ${str:0:1} == "/" ]] ; then
  echo 1;
else
  echo 0;
fi
```

This is going to take a substring of **str** starting at the **0th** character with length **1**.

#### Using Regex

```bash
if [[ $str =~ ^/ ]];then
  echo 1;
else
  echo 0;
fi
```

`^` indicates starting with.

### Remove `space` from a string

```bash
str=" This sentence contains leading trailing and intermediate whitespaces "

## Remove leading whitespaces
lstr="$( echo "${str}" | sed -e 's/^[[:space:]]*//')"
echo "$lstr"

## Remove trailing whitespaces
tstr="$( echo "${str}" | sed -e 's/[[:space:]]*$//')"
echo "$tstr"

## Remove all white spaces
astr="$( echo "${str}" | tr -d '[:space:]')"
echo "$astr"
```

### Check if a string/line exist in a file

```bash
if grep -Fxq "$WhatToFind" my_list.txt
then
    # code if found
else
    # code if not found
fi
```

Here,

* F: Affects how PATTERN is interpreted \(fixed string instead of a regex\)
* x: Match whole line
* q: Shhhhh... minimal printing

Example:

```bash
if grep -q ^"$prefix:" out.yaml; then
    echo "$prefix found in out.yaml"
else
    echo "Not found"
fi
```

This check if `out.yaml` file has a line starting with `$prefix`.

### Replace whole line in of a file that match a pattern

```bash
sed -i "/string or regex goes here/c\replace with this" fileName

```

Here, `-i` for replace in place and `c` is for change/replace.

Example:

```bash
sed -i "/^$key:/c\\$line" out.yaml
```

This replace the line that start with `$key` of `out.yaml` file with `$line`.

## Use Bearer Token

### With wget

```bash
wget --header="Authorization: Bearer ${TOKEN}" ${URL}
```

Example:

```bash
wget --header="Authorization: Bearer ${TOKEN}" https://sandbox.xill.io/v2/contents/5a184d0207903113023b5aaa/SANDBOX.md --no-check-certificate
```

### With curl

```bash
curl -H 'Accept: application/json' -H "Authorization: Bearer ${TOKEN}" ${URL}
```

Example:

```bash
curl -H 'Accept: application/json' -H "Authorization: Bearer ${TOKEN}" https://{hostname}/api/myresource
```

## Work with Executable

### Check an executable file static build or dynamic build

Use this command to check if a file is statistically linked or dynamically linked.

```bash
$ ldd ./yq
	not a dynamic executable
```

or

```bash
$ file yq
yq: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, with debug_info, not stripped
```



