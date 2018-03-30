.. ENCODE RNA-seq pipeline documentation master file, created by
   sphinx-quickstart on Fri Mar 30 10:43:26 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to ENCODE RNA-seq pipeline's documentation!
===================================================
This code runs the ``ENCODE RNA-seq pipeline``.

.. image:: https://www.genome.gov/images/feature_images/encode_logo.gif
   :alt: Link to encode_logo
   :align: center
   :target: https://www.encodeproject.org

:Version:
  |version|
:Authors:
  `ENCODE Data Coordinating Center <https://www.encodeproject.org>`_
:License:
  MIT
:Website:
  https://github.com/ENCODE-DCC/rna-seq-pipeline/

Example posting output to the portal:

.. code-block:: bash
          
  python accession_analyses.py ENCSR000123 \
      --assembly GRCh38 --force_patch=t

      
Example writing a function in Python:

.. code:: python

  def my_function():
      pass


Requirements
++++++++++++

You will need:

* ``Python=2.7``
* ``Docker``


.. _quickstart:

Quickstart
++++++++++++

In a nutshell, using ``ENCODE RNA-seq pipeline`` consists of these simple steps:

#. You write (or generate) a simple template of your script based on arguments your script is supposed to accept.
#. You run the ``create_mapping`` script (located in the package's ``DNAnexus`` directory) on it to get the fully functional script.

Eventually, you may want to add/remove/rename arguments your script accepts.
In that case, you just need to edit the script --- you don't need to repeate the two steps listed above!
Why? It is so because the script retains the template section, so if you need to make adjustments to the template, you just edit the template section of the script and run ``ENCODE RNA-seq pipeline`` on top of the script to get it updated.

How to use the ``ENCODE RNA-seq pipeline``?
You can either

* download and install it locally,
* use the `online generator <https://github.com/ENCODE-DCC/chip-seq-pipeline>`_, or
* use the `Docker container <https://github.com/ENCODE-DCC/chip-seq-pipeline>`_.

Generating WDL
+++++++++++++++++++++

``ENCODE RNA-seq pipeline`` features the ``wdl-init`` script that you can use to generate a template in one step.
Assume that you want a script that accepts one (mandatory) positional argument ``positional-arg`` and two optional ones ``--option`` and ``--print``, where the latter is a boolean argument.

In other words, we want to support these arguments:

* ``--option`` that accepts one value,
* ``--print`` or ``--no-print`` that doesn't accept any value, and
* an argument named ``positional-arg`` that we are going to refer to as positional that must be passed and that is not preceeded by *options* (such as ``--foo``, ``-f``).

We call ``ENCODE RNA-seq pipeline`` and as the desired result is a script, we directly pipe the output of ``wdl-init`` to ``ENCODE RNA-seq pipeline``:

.. code-block:: bash
          
  python accession_analyses.py ENCSR000123 \
      --assembly GRCh38 --force_patch=t

We didn't have to do much, yet the script is pretty capable.

Limitations
+++++++++++

.. warning::

  Please read this carefully.

#. The square brackets in your script have to match (i.e. every opening square bracket ``[`` has to be followed at some point by a closing square bracket ``]``).

   There is a workaround --- if you need constructs s.a. ``red=$'\e[0;91m'``, you can put the matching square bracket behind a comment, i.e. ``red=$'\e[0;91m'  # match square bracket: ]``.

This limitation does apply only to files that are processed by  ``ENCODE RNA-seq pipeline`` --- you are fine if you have the argument parsing code in a separate file and you don't use the ``INCLUDE_PARSING_CODE`` macro.
   You are also OK if you use ``wdl-init`` in the *decoupled mode*.

#. The generated code generally contains bashisms as it relies heavily on ``bash`` arrays to process any kind of positional arguments and multi-valued optional arguments.
   That said, if you stick with optional arguments only, a POSIX shell s.a. ``dash`` should be able to process the ``ENCODE RNA-seq pipeline``-generated parsing code.

   
API documentaiton
=================
.. image:: https://img.shields.io/badge/ENCODE-RNA--seq%20pipeline-blue.svg
           
.. automodule:: types
   :members:

      
Contents
==========

.. toctree::
   :maxdepth: 2
             
   tutorials
   howto
   reference


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
