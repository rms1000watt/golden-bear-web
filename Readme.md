# Golden Bear Web

## Introduction

Website for [Golden Bear]()

## Contents

- [Install](#install)
- [Build](#build)
- [Deploy](#deploy)
- [Credits](#credits)

## Install

```bash
sudo pip install jinja2 jinja2-cli aws
npm install -g csso uglifyjs html-minifier
```

## Build

```bash
buildHash=$(uuidgen | tr '[:upper:]' '[:lower:]' | tr -d '-' | cut -c 1-12)
echo '{"build_hash":"'"$buildHash"'"}' > index.json

# Handle CSS
rm assets/css/min-*.css
sed '/font-awesome.min.css/d' < assets/css/main.css > assets/css/tmp.css
cat assets/css/tmp.css assets/css/font-awesome.min.css assets/css/noscript.css | csso > "assets/css/min-$buildHash.css"
rm assets/css/tmp.css

# Handle JS
rm assets/js/min-*.js
uglifyjs --compress --mangle -o "assets/js/min-$buildHash.js" -- assets/js/*.js

# Handle HTML
jinja2 index.html.j2 index.json > tmp.html
html-minifier --collapse-boolean-attributes --collapse-whitespace --decode-entities --html5 --process-conditional-comments --remove-attribute-quotes --remove-comments --remove-empty-attributes --remove-optional-tags --sort-attributes --sort-class-name --trim-custom-fragments --use-short-doctype tmp.html > index.html
rm tmp.html
```

## Deploy

```bash
export AWS_PROFILE=golden-bear
aws s3 cp   index.html  s3://goldenbear.io/index.html  --acl public-read
aws s3 cp   favicon.ico s3://goldenbear.io/favicon.ico --acl public-read
aws s3 sync assets/     s3://goldenbear.io/assets/     --acl public-read
aws s3 sync images/     s3://goldenbear.io/images/     --acl public-read
```

## Credits

- `Overflow` by [HTML5 UP](https://html5up.net/)
