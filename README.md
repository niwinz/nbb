# Nodashka

A [babashka](https://babashka.org/)-like tool for Node.js.

## Status

Experimental.

## Build

Prequisites:

- [babashka](https://babashka.org/) >= 0.4.0
- [Clojure CLI](https://clojure.org/guides/getting_started#_clojure_installer_and_cli_tools) >= 1.10.3.933
- Node.js 16.5.0 (lower version may work, but this is the one I used to build)

To build:

- Clone and cd into this repo
- `bb release`
- `npm install -g`

## Usage

Install some NPM libraries to use in the script. E.g.:

```
$ npm install csv-parse
$ npm install shelljs
```

and suppose you have some local JS file `foo.js` with `console.log("hello");`.

Create a script which uses the NPM libraries:

``` clojure
(ns script
  (:require ["./foo.js"]
            ["csv-parse/lib/sync.js" :default csv-parse]
            ["fs" :as fs]
            ["shelljs" :default sh]))

(println (count (str (.readFileSync fs "script.cljs"))))

(prn (.ls sh "."))

(prn (csv-parse "foo,bar"))
```

Call the script:

```
$ nodashka script.cljs
hello
264
#js ["CHANGELOG.md" "README.md" "bb.edn" "deps.edn" "main.js" "node_modules" "out" "package-lock.json" "package.json" "shadow-cljs.edn" "src" "test.cljs"]
#js [#js ["foo" "bar"]]
```

The script takes about 200ms seconds to run on my laptop.

## Reagent

Nodashka includes `reagent.core` which will be lazily loaded when required. You
can use this together with [ink](https://github.com/vadimdemedes/ink) to create
a TUI application:

```
$ npm install ink
```

`ink-demo.cljs`:
``` clojure
(ns ink-demo
  (:require [reagent.core :as r]
            ["ink" :refer [render Text]]))

(defonce state (r/atom 0))
(doseq [n (range 1 11)]
  (js/setTimeout #(swap! state inc) (* n 500)))

(defn hello []
  [:> Text {:color "green"} "Hello, world! " @state])

(render (r/as-element [hello]))
```

<img src="img/ink.gif"/>

## License

Copyright © 2019-2020 Michiel Borkent

Distributed under the EPL License. See LICENSE.
