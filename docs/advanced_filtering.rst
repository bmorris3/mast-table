Advanced Filtering
==================

Suppose you are browsing a source catalog with ``mast-table``.  In :ref:`getting-started`, we
used the ``mast-table`` :ref:`getting-started-filters` interface 
to limit the visible table rows based on their values in one or more columns. 

In this example, we will apply custom, complex filters to the source catalog, including derived
parameters.

Query the Gaia DR3 catalog
--------------------------

First we query MAST for sources from the Gaia DR3 source catalog.

.. code-block:: python

    import astropy.units as u
    from astropy.coordinates import SkyCoord

    from mast_table import MastTable
    from astroquery.mast import Catalogs

    coord = SkyCoord(ra=9.701, dec=40.76, unit=u.deg)

    gaia_catalog = Catalogs().query_region(
        coordinates=coord,
        radius=30 * u.arcmin,
        catalog='GaiaDR3'
    )


.. image:: https://github.com/spacetelescope/widget-docs/blob/main/mast-table/custom_filters/query-result.jpg?raw=true
    :width: 100%
    :align: center
    :class: outlined


The query result has 19,316 sources (rows) and 152 columns. Descriptions for all columns in the
Gaia DR3 source catalog can be found in the MAST 
`Catalog Schema Browser <https://mast.stsci.edu/schema_browser/#/gaiadr3/gaia_source>`_.

Filter on derived parameters
----------------------------

Let's filter the table on each source's _absolute_ magnitude in G, which is not
available in the Gaia DR3 catalog columns. However, Gaia DR3 catalog results contain columns
for ``'parallax'`` and the G band apparent magnitude ``'phot_g_mean_mag'``. We will use these
parameters to compute the absolute magnitude:

.. code-block:: python

    import numpy as np
    from astropy.coordinates import Distance

    apparent_mag = gaia_catalog['phot_g_mean_mag']  # [mag]

    # fill NaNs in parallax with a special value, corresponds to a
    # distance of 999 pc:
    parallax = gaia_catalog['parallax'].filled(1000 / 999)  # [milliarcsec]

    # use astropy to convert from parallax to distance
    distance = Distance(parallax=parallax, allow_negative=True)

    absolute_mag = apparent_mag - 5 * np.log10(distance / (10 * u.pc)) * u.mag

We add a masked column of absolute magnitudes to the catalog table, masking magnitudes containing
NaNs. We reorder the columns so that absolute magnitude appears first, and then we display the
result with `~mast_table.mast_table.MastTable`:

.. code-block:: python

    from astropy.table import MaskedColumn

    # save the existing column names for later
    other_columns = gaia_catalog.colnames

    # add a masked column for the absolute magnitude
    gaia_catalog['abs_G_mag'] = MaskedColumn(
        data=absolute_mag,
        mask=np.isnan(absolute_mag)  # mask NaNs
    )

    # reorder catalog columns to show abs G mag first
    gaia_catalog = gaia_catalog[['abs_G_mag'] + other_columns]

    mast_table = MastTable(gaia_catalog)
    mast_table.show()

We can now construct a filter to view sources with absolute G magnitudes
fainter than 9.5 like so:

.. image:: https://github.com/spacetelescope/widget-docs/blob/main/mast-table/custom_filters/absolute-mag.jpg?raw=true
    :width: 100%
    :align: center
    :class: outlined

Then, we click Apply Filter.

Complex filters
---------------

The Filters widget UI can become cumbersome for combinations of many filter criteria.
In those cases, we suggest using the Python API in the Jupyter notebook to index
the `astropy.table.Table` directly before constructing the `~mast_table.mast_table.MastTable`.
For example, the code below displays a `~mast_table.mast_table.MastTable` widget
containing sources that meet the following criteria:

* proper motions less than 100 milliarcseconds per year
* **and** effective temperatures less than 5000 K
* **and** are **not**: galaxy candidates **or** quasar candidates

.. code-block:: python

    filter_criteria = (
        (gaia_catalog['pm'].quantity < 100 * u.mas / u.year) &
        (gaia_catalog['teff_gspphot'].quantity < 5000 * u.K) &
        np.logical_not(
            gaia_catalog['in_galaxy_candidates'] |
            gaia_catalog['in_qso_candidates']
        )
    )

    mast_table = MastTable(gaia_catalog[filter_criteria])
    mast_table.show()

With this "pre-filtered" table, you can experiment with adding additional filters
in the UI.
