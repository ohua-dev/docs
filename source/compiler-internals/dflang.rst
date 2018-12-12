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
               = some.package/sf (* function reference *)
                 <0> (* call site id *)
                 [s] (* state source (optional) *)
                 (a, b, c) (* arguments *) in
   let (y, z) = dataflow other.package/a_dataflow_fn<1>() in
   x
.. _target bindings:

Target Bindings
    A tuple of bindings that are created by this function call. The names
    should be unique in the expression. Always has to be a tuple, i.e.
    comma separated and surrounded by parentheses, even if only one value is
    created. Can be empty.

Function Reference
    The fully qualified reference to the function being called. The syntax is
    ``namespace/functionName``, i.e. ``ohua.lang/id``. May optionally be
    preceded by the keyword ``dataflow`` to indicate that this is not a simple
    function but a dataflow operator, i.e. translates to the ``nodeType`` being
    ``OperatorNode``.

Call Site Id
    A positive integer uniquely identifying this call. Uniqueness is not checked
    but the behavior of the compiler is undefined if call site id's are not
    unique.

State Source
    A value to be used as initial state for this function. Must be specified for
    stateful functions and omitted for stateless functions. Can be a literal of
    binding (same as :ref:`arguments <arguments>`).

.. _arguments:

Arguments
    A tuple of arguments as input to the function. As with :ref:`target bindings
    <target bindings>` these need to be comma separated and surrounded by
    parentheses, but can be empty. In addition to local variables, such as
    ``a``, ``b``, ``myVar``, numeric literals ``1``, ``0``, ``-4``, the unit
    literal ``()`` and references to environment expressions are allowed as
    arguments. The latter are positive numbers prefixed with ``$``, i.e. ``$1``,
    ``$100``.
