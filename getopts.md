##getopts demo

sample code:
```
#! /bin/bash

# A POSIX variable
OPTIND=1         # Reset in case getopts has been used previously in the shell.

# Initialize our own variables:
AUTOJUDGE=LATEST
CTL=LATEST
DA=LATEST

while getopts ":hd:c:a:" opt; do
    case "$opt" in
     d)  DA=$OPTARG
         ;;
     c)  CTL=$OPTARG
         ;;
     a)  AUTOJUDGE=$OPTARG
         ;;
     h)  usage 
         ;;
     \?) echo "Unknown option: -$OPTARG" >&2
         exit 1
         ;;
     :)  echo "Missing option argument for -$OPTARG" >&2
         exit 1
         ;;
     *)  echo "Unimplemented option: -$OPTARG" >&2
         exit 1
         ;;
     esac
done

shift $((OPTIND-1))

[ "$1" = "--" ] && shift

BASE_BIN=$(cd $(dirname ${0})>/dev/null;pwd)
WORKER_BASE=${BASE_BIN%/*}
WORKER_LIB=${WORKER_BASE}/lib

echo "DA(${DA}), CTL(${CTL}), AUTOJUDGE(${AUTOJUDGE}) will be synced to ${WORKER_LIB}"
```

some notes:

  - ":" : the preceding colon means disabling verbose mode
  - "d:" : this means "-d" should be followed by an argument, like "-d abc"