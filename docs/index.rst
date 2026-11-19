.. |rrn| raw:: html

  <a href="https://roman.science.stsci.edu" target="_blank">Roman Research Nexus</a>

mast-table
==========

``mast-table`` is an interactive table widget for Jupyter notebooks for browsing
query results from the `Mikulski Archive for Space Telescopes (MAST) <https://mast.stsci.edu>`_
at `Space Telescope Science Institute <https://www.stsci.edu>`_. 

The source code is available on `GitHub <https://github.com/spacetelescope/mast-table>`_.


Key Features
************

* **Runs on the** |rrn| or on your local machine in a Jupyter notebook
* **Browse MAST query results** for observations or sources in catalogs with a user interface 
  with a similar look and feel to the MAST `Mission Search <https://mast.stsci.edu/search>`_ query
  result page
* **Open footprints and data products** in `mast-aladin <https://mast-aladin.readthedocs.io/>`_ 
  and `jdaviz <https://jdaviz.readthedocs.io/en/stable/>`_
* **Filter down** long query results to the results you need


Demo
----

Watch as we:

1. Query MAST for JWST observations in a notebook
2. Display observations with ``mast-table``
3. Select an observation, query for its data products
4. Display data products with ``mast-table``
5. Display observation footprints with `mast-aladin <https://mast-aladin.readthedocs.io/>`_

.. raw:: html

   <video autoplay muted loop playsinline controls width="100%">
     <source src="https://stsci.box.com/shared/static/21ftadpbexlkobkc18pdobrwh7ql88c2.mp4" type="video/mp4">
     Your browser does not support the video tag.
   </video>

Contents
********

.. toctree::
   :maxdepth: 2

   installation
   getting_started
   catalogs
   mast_aladin
   api



