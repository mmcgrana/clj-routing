`clj-routing` is a library for implementing fast, 2-way URL routing in Clojure web applications.

The library allows you to separate the core routing implementation (handled by `clj-routing`) from the interface or DSL for describing routes (typically handled by a web framework).

Usage
-----

    (use 'clj-routing.core)
    
    (def routes
      [[:index-bars  :get "/foo/bars"                         ]
       [:show-bar    :get "/foo/bars/:id"                     ]
       [:create-bat  :put "/foo/bars"                         ]
       [:with-conds  :get "/foo/:action" {:action "biz|bats"} ]
       [:catch-all   :any "/:path"       {:path   ".*"}       ]])
    
    (def generate  (compiled-generator  routes))
    (def recognize (compiled-recognizer routes))
    
    (generate :index-bars)
    => [:get "/foo/bars" nil]
    
    (generate :show-bar {:id "theid"})
    => [:get "/foo/bars/theid" {}]
    
    (generate :show-bar {:id "theid" :more "extra"})
    => [:get "/foo/bars/theid" {:more "extra"}]
    
    (generate :show-bar)
    => Exception: "Missing param: :id"
    
    (generate :foo-bar)
    => Exception: "Unrecognized route name: :foo-bar" 
    
    (recognize :get "/foo/bars")
    => [:index-bars {}]
    
    (recognize :get "/foo/bars/bat")
    => [:show-bar {:id "bat"}]
    
    (recognize :get "/foo/biz")
    => [:with-conds {:action "biz"}]
    
    (recognize :get "/foo/fiz")
    => [:catch-all {:path "foo/fiz"}]
    
    (recognize :foo "/foo/bars")
    => Exception: "Unrecognized method: :foo"

Please see the `clj-routing.core` docstring and the `clj-routing.core-test` code for additional documentation.

License
-------

Copyright (c) 2009 Mark McGranaghan and released under an MIT license.