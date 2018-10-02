.. _project-organisation:

Organisation of the ohua project
================================

.. _ohua-repositories:

Repositories
------------

ohua-core
    https://github.com/ohua-dev/ohua-core

    This is the heart of the compiler. The core library that defines the
    intermediate language, the dataflow graph, transitions, optimisations, hooks
    etc.

ohuac
    https://github.com/ohua-dev/ohuac

    The standalone compiler. An executable which combines ohua :ref:`frontends` and
    core to parse algorithm files and generate code for various target platforms.
