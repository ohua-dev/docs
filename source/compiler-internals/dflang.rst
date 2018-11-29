The dataflow language ``DFLang``
================================

The dataflow language is the bridge between the call by need lambda calculus and
a dataflow graph. It retains aspects of both but is neither completely. It is
represented as a sequence of ``let`` assignments, where the right hand side of
the assignment is always a call of a stateful function or dataflow operator.
Multiple arguments can be given to this call, however they must always be
references to either a locally (``let``) bound value, or an environment reference.

It is thus much more restricted than the :ref:`algorithm language` because it
does not support lambdas, and the RHS of ``let`` is not an arbitrary expression.

The dataflow language is defined in ``Ohua.DFLang.Lang``.


Writing ``DFLang``
------------------

It is sometime necessary (in particular for tests) to create ``DFLang`` values.
To avoid having to write them directly in Haskell, the ``ohua-test-tools``
library includes a ``DFLang`` parser. This parser can be used in Haskell code
when the ``QuasiQuotes`` extension is enabled by importing ``Ohua.Test`` and
using the ``[embedDflang| Your dflang expression |]`` quasi quoter.

It supports the same style comments as the algorithm language.

An example:

.. code-block:: ocaml

   let (x) (* target bindings *)
               = some.package/sf (* function ref *)
                 <0> (* call site id *)
                 (a, b, c) (* arguments *) in
   let (y, z) = dataflow other.package/a-dataflow-fn<1>() [a] (* context arg (optional) *) in
   x

In addition to local variables, such as ``a``, ``b``, ``myVar`` the arguments to
functions can also be numeric literals ``1``, ``0``, ``-4`` or references to
environment expressions. The latter are positive numbers prefixed with ``$``,
i.e. ``$1``, ``$100``.
