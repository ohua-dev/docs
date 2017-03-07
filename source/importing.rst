Importing syntax
================

Stateful functions for use in the ``ohua`` macro are being brought into scope with a macro called ``com.ohua.lang/ohua-require``.
The ``ohua-require`` macro scans the classpath for the specified functions, loads them and makes them available for use with ``ohua`` in the namespace in which it was called.
It supports qualified and unqualified imports as well as aliasing for both namespaces/packages and functions.

.. important::
    Importing stateful functions like this makes them available *only* for use in the ``ohua`` macro.
    They will not be available via the standard clojure symbol resolution.
    If you *must* resolve the functions yourself use ``com.ohua.link/resolve``.

The syntax for ``ohua-require`` is intentionally similar to ``clojure.core/require``.

::

    (ohua-require 
        [my.package]
        [my.package :as package]
        [my.package :refer [function]]
        [my.package :refer :all])

The macro takes a lists or vectors as input.
The first element of each is the name of a package/namespace which should be imported.
Functions from the package are then available qualified as ``my.package/function``.
Optionally you may alias the package using the ``:as`` keyword and providing a new name to bind the package to.
And lastly you may use ``:refer`` to specify functions which should be imported such that they may be used unqualified.
``:refer :all`` makes all functions from the package available as unqualified imports.
These are the only ``require`` semantics which are currently implemented.
