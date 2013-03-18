=================
Benchy
=================
A lightweight benchmarking framework written in Python focused on performing
 memory consumption and runtime performance comparison for Python programs.

THe goal of this framework is to help scientific developers to perform
benchmarkings of several algorithmical approaches written in Python.

It's has the `memory_profiler <http://pypi.python.org/pypi/psutil>`_ ,
`numpy <http://pypi.python.org/pypi/psutil>`_  and
`matplotlib <http://pypi.python.org/pypi/psutil>`_ as dependencies.


==============
 Installation
==============
To install through easy_install or pip::

    $ easy_install -U benchy # pip install -U benchy

To install from source, download the package, extract and type::

    $ python setup.py install


=======
 Usage
=======
To use the benchy framework, you must first define the functions you would
like to benchmark. In this example, we create three versions of a simple
function ``create_list`` that allocates the list ``a`` with 100000 elements::

    from benchy.api import Benchmark

    common_setup = ""
    statement = "lst = ['i' for x in range(100000)]"
    benchmark1 = Benchmark(statement, common_setup, name= "range")

    statement = "lst = ['i' for x in xrange(100000)]"
    benchmark2 = Benchmark(statement, common_setup, name= "xrange")

    statement = "lst = ['i'] * 100000"
    benchmark3 = Benchmark(statement, common_setup, name= "range")


With all benchmarks created, we could test a simple benchmark by
calling the method ``run``::

    print benchmark1.run()

The output will follow the structure below::

    {'memory': {'repeat': 3,
                'success': True,
                'units': 'MB',
                'usage': 2.97265625},
     'runtime': {'loops': 100,
                 'repeat': 3,
                 'success': True,
                 'timing': 7.5653696060180664,
                 'units': 'ms'}}


The dict associated to the key *memory* represents the memory performance
results. It gives you the number of calls *repeat* to the statement, the average
consumption *usage* in *units* . In addition, the key 'runtime' indicates
the runtime performance in timing results. It presents the number of calls
*repeat* following the average time to execute it *timing* in *units*.

Now let's check which one is faster and which one consumes less memory. Let's
create a ``BenchmarkSuite``. It is referred as a container for benchmarks.::

     from benchy.api import BenchmarkSuite
     suite = BenchmarkSuite()
     suite.append(benchmark1)
     suite.append(benchmark2)
     suite.append(benchmark3)

Finally, let's run all the benchmarks together with the ``BenchmarkRunner``.
This class can load all the benchmarks from the suite and run each individual
analysis and print out interesting reports::

    from benchy.api import BenchmarkRunner
    runner = BenchmarkRunner(benchmarks=suite, tmp_dir='.', name= 'List Allocation Benchmark')


Let's run the suite::

    runner.run()

Output will follow::


============================
 Frequently Asked Questions
============================
    * Q: How accurate are the results ?
    * A: This module gets the memory consumption by querying the
      operating system kernel about the amount of memory the current
      process has allocated, which might be slightly different from
      the amount of memory that is actually used by the Python
      interpreter. Also, because of how the garbage collector works in
      Python the result might be different between platforms and even
      between runs.

    * Q: Does it work under windows ?
    * A: Yes, but you will need the
      `psutil <http://pypi.python.org/pypi/psutil>`_ module.



===========================
 Support, bugs & wish list
===========================
Send issues, proposals, etc. to `github's issue tracker <https://github.com/python-recsys/benchy/issues>`_ .

If you've got questions regarding development, you can email me
directly at marcel@pingmind.com


=============
 Development
=============
Latest sources are available from github:

    https://github.com/python-recsys/benchy


=========
 Authors
=========
This module was written by `Marcel Caraciolo <http://aimotion.blogspot.com>`_

Inspired by Wes Mckinney `vbench <https://github.com/pydata/vbench>`_.


=========
 License
=========
Simplified BSD