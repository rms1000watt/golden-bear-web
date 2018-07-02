# Golden Bear Web

## Introduction

Website for [Golden Bear]()

## Contents

- [Build](#build)
- [Deploy](#deploy)
- [Credits](#credits)

## Build

```bash
# Handle CSS
sed 's|@import url(.*||g' < assets/css/main.css > assets/css/tmp.css
cat assets/css/tmp.css assets/css/font-awesome.min.css assets/css/noscript.css assets/css/source-sans-pro.css | csso > assets/css/min.css
rm assets/css/tmp.css

# Handle JS
uglifyjs --compress --mangle -o assets/js/min.js -- assets/js/*.js
```

## Deploy

## Credits

- `Overflow` by [HTML5 UP](https://html5up.net/)
