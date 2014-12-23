#!/bin/bash

# Framework for invoking OpenAura API 
# http://developer.openaura.com/examples/
#
# c. 2014 Lucas Gonze. Hereby in public domain.

config=`dirname "$0"`/config.sh
if [ -f "$config" ]
	then
	YOUR_API_KEY=`egrep '^YOUR_API_KEY=' "$config" | awk -F= '{print $2}'`
fi
if [ "$YOUR_API_KEY" == "" ]
	then	
	echo "First set API key in $config" >&2
	exit 1
fi

# Taylor Swift, but of course.
ARTIST_ID=47

# temp file for pretty printing JSON
scratch=$(mktemp -t openaura-tmp.XXXXXXXXXX)
touch "$scratch"

function getter {
	(curl -v -L -XGET "http://api.openaura.com/$1&api_key=$YOUR_API_KEY" > "$scratch") || exit 1
	node -e "console.log(JSON.stringify(JSON.parse(require('fs').readFileSync(process.argv[1])), null, 4));" "$scratch"
}

function finish {
	echo "finish: rm $scratch"
	rm -rf "$scratch"
}
trap finish EXIT


ACTION="$1"

case "$ACTION" in
"PARTICLES")
	getter "v1/particles/artists/$ARTIST_ID?limit=10&id_type=oa:artist_id" | less	
  ;;
*)
  echo "usage: $0 [ACTION]"
  ;;
esac

exit

