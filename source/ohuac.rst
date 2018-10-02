.. _ohuac:

The standalone compiler: ``ohuac``
==================================

The ``ohuac`` executable is a standalone program which can compile a source file
written in one of the ohua :ref:`frontends` and :ref:`generate code
<code-gen-targets>` for various platforms.

.. _installing-ohuac:

Getting it
----------

Prebuilt binaries are available for 64bit Linux and OSX. Provided for each
release by `Travis CI <https://travis-ci.org/ohua-dev/ohuac>`_.

Simply download the appropriate executable for you platform (``ohuac-linux`` or
``ohuac-osx``) from the `releases page`_.

.. _releases page: https://github.com/ohua-dev/ohuac/releases/latest

.. _ohuac-source:

Building from source / Getting the source code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The source code for the compiler can be `found on GitHub
<https://github.com/ohua-dev/ohuac>`_. However major parts of the compiler are
separate libraries. For more information on the repository structure see the
:ref:`project organisation page <ohua-repositories>`.

``ohuac`` is written in Haskell and uses the `stack tool
<https://docks.haskellstack.org>`_ for building and dependency management.
If you have stack installed already you can simply download the source from the
`releases page`_ and run ``stack install``.

This downloads and builds all dependencies, as well as the Haskell compiler
``ghc``, should it not be present already. It should not interfere with any
system-level libraries, Haskell ecosystems etc.

It builds the executable ``ohuac`` and copies it to ``~/.local/bin``. If you do
not wish this use ``stack build`` instead and find the path of the finished
binary using ``stack exec -- which ohuac`` afterwards. After building the binary
can be freely moved to any location on the system.

Usage
-----

The capabilities and options for the compiler can be interactively explored by
calling ``ohuac --help`` or ``ohuac COMMAND --help`` to get help for a specific
``COMMAND``.

The ``build`` command
^^^^^^^^^^^^^^^^^^^^^

The most common command is ``build``.

The build command transitively builds your ohua modules. This means it not only
compiles the one module you directly specify but also all modules it may depend
on.

Currently the search path for modules cannot be influenced. Therefore they must
be in a specific location, which is ``module/submodule.ohuac``. The type of file
(``.ohuac`` or ``.ohuas``) does not matter in this case. In fact they can also be
mixed. A ``ohuas`` file can import a module that is defined as a ``ohuac`` file.

As an example we may have a module ``A.ohuac``, which depends on ``foo.bar.B``
and ``foo.C``.

.. code-block:: none

    A
    |-- foo.bar.B
    '-- foo.C

During compilation the compiler will look for these dependencies
at ``foo/bar/B.ohuac`` or ``foo/bar/B.ohuas`` and ``foo/C.ohuac`` or
``foo/C.ohuas``. All paths being relative to the current working directory.
So if the compiler was called with ``ohuac build A.ohuac``, the working
directory is expected to look something like this

.. code-block:: none

    WORKING_DIR/
         |-- A.ohuac
         '-- foo/
              |--bar/
              |   '-- B.ohuac
              '-- C.ohuas


If both an ``.ohuac`` and ``.ohuas`` file is found for a particular dependency
the compiler will exit with an error.


Universal options
^^^^^^^^^^^^^^^^^

The options listed in ``ohuac --help`` apply to all subcommands. They are
generally **prepended** to the options passed to the subcommand.

.. _code-gen-targets:

Supported targets for code generation
-------------------------------------
