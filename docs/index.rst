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


Requirements
++++++++++++

To run the pipeline locally you need:

* ``git clone https://github.com/ENCODE-DCC/rna-seq-pipeline.git``
* Java 8
* `Cromwell, the WDL runner <http://cromwell.readthedocs.io/en/develop/tutorials/FiveMinuteIntro/>`_
* `Docker <https://docs.docker.com/install/>`_

To run the pipeline on DNAnexus cloud you need:

* ``git clone https://github.com/ENCODE-DCC/rna-seq-pipeline.git``
* Java 8+
* `dx-toolkit <https://wiki.dnanexus.com/downloads>`_
* python 2.7
* Account to DNAnexus (note, computation is not free)
* `DxWDL compiler <https://github.com/dnanexus/dxWDL>`_


.. _quickstart:

Quickstart
++++++++++++


Generating WDL
+++++++++++++++++++++



Limitations
+++++++++++


      
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
