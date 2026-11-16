Catalogs
========


Load a catalog
--------------

In addition to observation and product query results, `~mast_table.mast_table.MastTable` also
supports catalog query results. 

Let's query `MAST Catalogs with astroquery 
<https://docs.astropy.org/projects/astroquery/en/latest/mast/mast_catalog.html>`_ for sources
in a region of interest, and show the results with `~mast_table.mast_table.MastTable`.

.. code-block:: python

    import astropy.units as u
    from astropy.coordinates import SkyCoord

    from mast_table import MastTable
    from astroquery.mast import Catalogs

    coord = SkyCoord(ra=9.701, dec=40.76, unit=u.deg)
    gaia_catalog = Catalogs().query_region(
        coord, radius=1 * u.arcmin, catalog='GaiaDR3'
    )

    mast_table = MastTable(gaia_catalog)
    mast_table.show()

.. image:: https://github.com/spacetelescope/widget-docs/blob/main/mast-table/catalog.jpg?raw=true
    :width: 100%
    :align: center
    :class: no-click

|



Display columns
---------------

Catalogs often have more columns than could fit on your screen. Click the "Display columns" dropdown 
at the top of the widget to select a subset of columns to display.


.. image:: https://github.com/spacetelescope/widget-docs/blob/main/mast-table/catalog-display-columns.jpg?raw=true
    :width: 100%
    :align: center
    :class: no-click

|

Filters
-------

Suppose for this workflow, we're interested in targets with parallax greater than 0.1 milliarcseconds. First, let's
check the unit of the ``parallax`` column:

.. code-block:: python

    >>> gaia_catalog['parallax'].unit
    mas

Add a filter to see only sources with parallax > 0.1 mas. After clicking "Apply Filter", the ``filter_mask`` 
attribute of `~mast_table.mast_table.MastTable` will be updated with an array of booleans with the same
length as the table, where True values correspond to rows that meet the filter criteria.


.. image:: https://github.com/spacetelescope/widget-docs/blob/main/mast-table/catalog-filter.jpg?raw=true
    :width: 100%
    :align: center
    :class: no-click


Load into mast-aladin
---------------------

Now we view the sources on the sky with `mast-aladin <https://mast-aladin.readthedocs.io/>`_.

We slice the source catalog with the filtered sources using the ``filter_mask`` attribute from 
``mast-table``, and we view these sources in `~mast_aladin.app.MastAladin` with 
`~mast_aladin.app.MastAladin.add_table`.


.. code-block:: python

    from mast_aladin import MastAladin

    # launch mast-aladin centered on search coordinate
    mast_aladin = MastAladin(
        target=coord
    )

    # get the filtered (visible) rows from the catalog
    gaia_catalog_filtered = gaia_catalog[
        mast_table.filter_mask
    ]

    # add the filtered table to mast-aladin
    overlay = mast_aladin.add_table(
        table=gaia_catalog_filtered,
        color='#52ff80',
        shape='square',
        size=70
        
    )

    mast_aladin.show()

.. image:: https://github.com/spacetelescope/widget-docs/blob/main/mast-table/catalog-mast-aladin.jpg?raw=true
    :width: 100%
    :align: center
    :class: no-click

