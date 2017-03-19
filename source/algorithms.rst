.. _ algorithms:

Algorithms
==========

Algorithms are ohua's high level abstraction of a dataflow graph.

An algorithm combines stateful functions and other algorithms into a program which can then be executed by the ``ohua`` macro.
The ``ohua`` macro decomposes the algo into a dataflow graph which it executes in parallel.

.. _defining algorithms:

Defining algorithms
-------------------

The ``com.ohua.lang/algo`` macro allows one to define an algo using the Clojure programming language.
All functions which are used in the algo must be in scope in the current namespace.
See :ref:`ohua-require` for more information.


.. _ohua-require:

Bringing stateful functions into scope
--------------------------------------

.. _runtime execution model:

Runtime execution model
-----------------------


