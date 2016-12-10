Defining stateful functions
===========================

Defining stateful functions is currently supported for three languages: :ref:`Java <sfn-java>`, :ref:`Clojure <sfn-clojure>` and :ref:`Scala <sfn-scala>`.
In theory however any language can define a stateful function, so long as it creates JVM bytecode for a class and a method.

.. _sfn-java:

Stateful functions with Java
----------------------------

Defining stateful functions in Java is very simple.
Every stateful function needs an associated class.

.. admonition:: The class must satisfy the following conditions

    #. The class must be ``public``. 
        This enables the runtime to find it.
    #. The class has a default constructor and is not ``abstract``.
        Since the runtime cannot know what the constructor arguments should be it will attempt to instantiate it with none.
    #. Only one stateful funciton is defined on the class.
        The class may define as many methods and members as it wants, however it may only define one stateful function.
        This restriction may be lifted in the future.
        However for the present if you wish to define multiple stateful functions in one file we suggest using static inner classes.
    #. If the class is an inner class it must be ``static``.
        Otherwise the runtime will be unable to instantiate the class.

To define the stateful function on the class itself simply annotate the desired method with ``@defsfn``.

.. code-block:: java

    public class AssociatedClass {

        @defsfn
        public String concat(String a, String b) {
            return a + b;
        }
    }

.. admonition:: The method must satisfy the following conditions

    #. The method must be ``public``. 
        Otherwise the runtime will not be able to call it.
    #. The method must not be ``static``.
        Using a static method will lead to an arity exception.
        If you must have a static version of your code define the static method and then define a second, non static method which calls the static one and annotate this one with ``defsfn``.


.. _sfn-clojure:

Stateful functions in Clojure
-----------------------------

Stateless Clojure functions
^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can use any normal clojure function in ohua. 
User defined functions as well as library functions can be directly called in the ohua EDSL.

.. warning:: As of yet there is no support for lambdas

    As an example, this does not work:
    ::

        (ohua
          (let [x (accept socket)
                lam (fn [y] ( ... x))]
            ..)


Stateful Clojure functions 
^^^^^^^^^^^^^^^^^^^^^^^^^^

Stateful functions in Clojure are simply Clojure functions, which have been annotated with the metadata ``:init-state``.
This ``:init-state`` metadata contains a Clojure expression which initializes the state for the stateful function.
This can be any Clojure expression and it may produce any Clojure data structure.
The *exact reference* returned by ``:init-state`` will be passed to every invokation of the stateful function.
Since this state reference will be passed to the function when invoked every Clojure stateful function must have as its first argument the reference for the state, usually called ``this``.
Therefore if you require mutable state, we recommend using clojure atoms, mutable Java data structures or mutable clojure data structures.

There is a convenience macro called ``defsfn`` which works like ``defn`` but additionally takes as a second argument the ``:init-state`` expression.
::

    ; defined with defsfn
    (defsfn my-funciton (new java.util.ArrayList) [this arg1]
      (if (> (.size this) 6)
        (let [copy (new java.util.ArrayList this)]
          ; mutable actions are allowed
          (.clear this)
          copy)
        (.add this arg1)))
    
    ; defined with defn
    (defn ^{:inti-state '(atom #{})} was-seen [this thing]
      (if (contains? @this thing)
        true
        (do
          (swap this conj thing)
          false)))


.. _sfn-scala:

Stateful functions in Scala
---------------------------

Defnining stateful functions in Scala is basically identical to defining stateful functions in Java.
See the requirements for the method and associated class in :ref:`How to define stateful functions in Java <sfn-java>`.
Annotate the method with ``@defsfn``.

.. code-block:: Scala

    class Concat {
        @defsfn
        def concat(a:String, b:String) -> String = {
            a + b
        }
    }

