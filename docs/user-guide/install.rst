Installation Guide
==================

Automatic Installation
----------------------

To install MXNet.jl, simply type

.. code-block:: julia

   Pkg.add("MXNet")

in the Julia REPL. Or to use the latest git version of MXNet.jl, use the following command instead

.. code-block:: julia

   Pkg.checkout("MXNet")

MXNet.jl is built on top of `libmxnet
<https://github.com/dmlc/mxnet>`_. Upon installation, Julia will try to
automatically download and build libmxnet.

The libmxnet source is downloaded to ``Pkg.dir("MXNet")/deps/src/mxnet``. The
automatic build is using default configurations, with OpenCV, CUDA disabled.
If the compilation failed due to unresolved dependency, or if you want to
customize the build, it is recommended to compile and install libmxnet manually.
Please see :ref:`below <manual-compilation>` for more details.


Manual Compilation
------------------

It is possible to compile libmxnet separately and point MXNet.jl to a the existing library in case automatic compilation fails due to unresolved dependencies in an un-standard environment; Or when one want to work with a seperate, maybe customized libmxnet.

To build libmxnet, please refer to `the installation guide of libmxnet
<http://mxnet.readthedocs.org/en/latest/build.html>`_. After successfully
installing libmxnet, set the ``MXNET_HOME`` environment variable to the location
of libmxnet. In other words, the compiled ``libmxnet.so`` should be found in
``$MXNET_HOME/lib``.

.. note::

   The constant ``MXNET_HOME`` is pre-compiled in MXNet.jl package cache. If you
   updated the environment variable after installing MXNet.jl, make sure to
   update the pre-compilation cache by ``Base.compilecache("MXNet")``.

When the ``MXNET_HOME`` environment variable is detected and the corresponding
``libmxnet.so`` could be loaded successfully, MXNet.jl will skip automatic building during installation and use the specified libmxnet instead.

Basically, MXNet.jl will search ``libmxnet.so`` or ``libmxnet.dll`` in the following paths (and in that order):

* ``$MXNET_HOME/lib``: customized libmxnet builds
* ``Pkg.dir("MXNet")/deps/usr/lib``: automatic builds
* Any system wide library search path

Note that MXNet.jl will not find ``libmxnet.so`` even if it is on the path if a library it depends upon is missing from the LD_LIBRARY_PATH.  Thus, if you are going to compile to add CUDA, the path to the CUDA libraries will have to be added to LD_LIBRARY_PATH.
