# Friends Graph

A simple demo using 
[fulcro](https://github.com/fulcrologic/fulcro3),
[pathom](https://github.com/wilkerlucio/pathom) and
[crux](https://github.com/juxt/crux)

## Running (for clj devs)

Install `npm` deps
```bash
$ npm install
```

Start a REPL with `dev` profile 

```bash
$ clj -A:dev
```

Call `(user/-main)` function. Wait the `shadow-cljs` build

Connect at [localhost:8080](http://localhost:8080)

## Running (for js devs)

Install `npm` deps
```bash
$ npm install
```

Start shadow-cljs via npm

```bash
$ npm start
```

Connect at [localhost:8080](http://localhost:8080)

##### React native (not working)

After get the web up'n'running, start the metro server

```bash
npm run rn-server
```

Then launch the app on emulator/app.

```bash
npm run rn-android
```

## Hacking

```
src/
├── dev
│   └── user.clj ## dev helper
└── main
    └── souenzzo
        └── graph_demo
            ├── client.cljs   ## fulcro sutff
            └── core.clj      ## http, pathom and crux stuff
deps.edn                      ## clj deps
package.json                  ## js deps
```

Fulcro components generate a query like this

```clojure
[{[:user/id "foo"]  [:user/id
                     :user/color
                     {:user/friends [:user/id 
                                     :user/color 
                                     {:user/friends [:user/id]}]}]}] 
```

Patohom resolver `souenzzo.graph-demo.core/friends` (bad name) 
will get `"foo"`, turn into `:user.id/foo`, get the entity from crux, usually something like
`{:crux.db/id "foo" :user/friends [:user.id/var]}` and return `{:user/friends [{:user/id "bar"}]}`

Pathom will flow the query recursively.
