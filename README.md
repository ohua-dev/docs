# Ohua Documentation

Eventually this will host the ohua documentation.
For now this is a collection of notes about the docs and its contents.

## Docs notes

- Host on [ReadTheDocs](https://readthedocs.org)?

## Content Notes

### Testing ohua with quoting (Linker related)

Tests are run in a different namespace in clojure, therefore if there is quoted ohua code in the test it may be necessary to create a fixture to import the ohua namespaces.

For instance in the following code:

```Clojure
(defn import-fixture [f]
  (link/ohua-require-fn '[com.ohua.lang.tests :refer :all])
  (f))

(clojure.test/use-fixtures :once import-fixture)


(deftest t
  (let [code ; Because of this quote the we need the fixture
             '(com.ohua.lang/ohua
                (let [input (accept)
                      algo-result ((com.ohua.lang/algo my-anon-algo [] (read)))]
                  (collect algo-result input)))
        t-code (walk/macroexpand-all code)]
    (l/enable-logging)
    (l/write t-code :dispatch clojure.pprint/code-dispatch)
    (ohua-test/compare-deep-code t-code '(let*
                                           [^:skip-comparison G__2870 (new com.ohua.lang.OhuaRuntime)]
                                           (. ^:skip-comparison G__2870 createOperator "accept" 100)
                                           (. ^:skip-comparison G__2870 createOperator "com.ohua.lang/algo-in-void" 101)
                                           (. ^:skip-comparison G__2870 createOperator "com.ohua.lang/algo-out" 102)
                                           (. ^:skip-comparison G__2870 createOperator "read" 103)
                                           (. ^:skip-comparison G__2870 createOperator "collect" 104)
                                           (. ^:skip-comparison G__2870 registerDependency 100 -1 104 1)
                                           (. ^:skip-comparison G__2870 registerDependency 101 -1 103 -1)
                                           (. ^:skip-comparison G__2870 registerDependency 102 -1 104 0)
                                           (. ^:skip-comparison G__2870 registerDependency 103 -1 102 0)
                                           (. ^:skip-comparison G__2870
                                              setArguments
                                              101
                                              (clojure.core/into-array
                                                com.ohua.lang.Tuple
                                                (clojure.core/list
                                                  (new
                                                    com.ohua.lang.Tuple
                                                    (clojure.core/int 0)
                                                    user/my-anon-algo))))
                                           (. ^:skip-comparison G__2870 execute {"shared-env-vars" {}})
                                           ^:skip-comparison G__2870)
                                 )
    )
  )
```