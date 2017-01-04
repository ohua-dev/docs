Stateful functions
==================

A stateful function is a piece of 'native' code which is sequentially executed by ohua and has for its execution context some associated, opaque state.

Concretely this means that a stateful function, in Java for instance, is a method and an associated class.

An example:

.. todo:: Better example?

.. code:: Java

    public class IntegerAccumulator {
        private int counter = 0;

        @defsfn
        public int increment(int valueToAdd) {
            counter += valueToAdd;

            return counter;
        }
    }

For each invokation site of the stateful function ohua creates a new instance of the associated class.
And each time that particular piece of code is run the same instance of the associated class is handed to the method.
