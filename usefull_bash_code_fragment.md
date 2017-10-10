#### convert relative path to absolute path

```
BASE_BIN=$(cd $(dirname ${0})>/dev/null;pwd)
BASE=${BASE_BIN%/*}
```

#### loop content line by line
With for and IFS:

```
#!/bin/bash

IFS=$'\n'       # make newlines the only separator
set -f          # disable globbing
for i in $(cat "$1"); do
  echo "tester: $i"
done
```

Or with read (no more cat):

```
#!/bin/bash

while IFS= read -r line; do
  echo "tester: $line"
done < "$1"
```

#### request a confirmation from user in order to continue

```
function confirm () {
    # call with a prompt string or use a default
    read -r -p "${1:-Are you sure? [y/N]} " response
    case $response in
        [yY][eE][sS]|[yY]) 
            true
            ;;
        *)
            false
            ;;
    esac
}

confirm "Are you sure?" && some following cmds.
```

#### use awk split string

```
echo "12|23|11" | awk '{split($0,a,"|"); print a[3],a[2],a[1]}'
```

#### enable core dump

one session

```
ulimit -c unlimited
```

#### check whether a certain port has been occupied or not.

```
lsof -i:PORT
```

#### delete all files but one

```
find . ! -name 'file_you_want_to_keep.txt' -type f -exec rm -f {} +
```

#### install epel

```
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
rpm -ivh epel-release-latest-6.noarch.rpm
```

####run command in remote

```
ssh test-2 "/opt/idagent/services/etl/carte.sh 127.0.0.1 8080 > ${logfile} 2>&1 &"
```

####check port

```
netstat -nap | grep pid  
```

####prompt in bash

```
echo "Do you wish to install this program?"
select yn in "Yes" "No"; do
    case $yn in
        Yes ) make install; break;;
        No ) exit;;
    esac
done
```

####check command exists

```
type foo >/dev/null 2>&1 || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
hash foo 2>/dev/null || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
```

####match file path

```
$dd =~ ^(/[^/ ]*)+/?$
```