# Ohua Documentation

Eventually this will host the ohua documentation.
For now this is a collection of notes about the docs and its contents.

## Installing

### Engine and building

Ohua's documentation uses the [Sphinx](http://www.sphinx-doc.org/) to generate html documentation from [reStructuredText](http://docutils.sourceforge.net/rst.html).

The Sphinx engine can be installed easily with `pip install Sphinx`.

Sphinx supports several different output types and there is a makefile provided for running the build.
For instance if we want to build the html version run `make html`.
The generated html will be in `build/html`.

Update to the [online hosted documentation](https://ohua.readthedocs.io/) simply push to the master branch of this repository.

### Theme

The ohua docs use the Sphinx theme provided by ReadTheDocs.
Instructions on how to install the theme can be found [here](https://github.com/snide/sphinx_rtd_theme).


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
  (let [code ; Because of this quote we need the fixture
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
