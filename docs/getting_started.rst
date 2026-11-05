Getting started
---------------

Query MAST
^^^^^^^^^^

In this example, we'll query MAST for JWST observations of a stellar merger
event. Let's use astroquery to search for JWST observations with 
`astroquery.mast.missions.MastMissionsClass`:

.. code-block:: python

    from astroquery.mast import MastMissions
    import astropy.units as u
    from astropy.coordinates import SkyCoord

    coord = SkyCoord(ra=9.701, dec=40.76, unit=u.deg)

    mission_search = MastMissions(mission='jwst')
    observations = mission_search.query_region(
        coordinates=coord,    # location of stellar merger
        radius=8 * u.arcmin   # cone search radius
    )


.. admonition:: Alternative: search via webform
   :class: tip

   To query MAST in your web browser, use
   `JWST Search <https://mast.stsci.edu/search/ui/#/jwst>`_.
   JWST Search will display query results as a table, and plot 
   the observation footprints on the Aladin full-sky atlas. 
   `Follow this link to see results for the above query 
   <https://mast.stsci.edu/search/ui/#/jwst/results?resolve=true&target=9.701+40.76&radius=8>`_.



Astroquery returns a `astropy.table.Table` object with the MAST query results.
We can interact with the results using ``mast-table``.

Launch mast-table
^^^^^^^^^^^^^^^^^

Initialize the `~mast_table.mast_table.MastTable` widget with:

.. code-block:: python

    from mast_table import MastTable

    mast_table = MastTable(observations)

    mast_table.show()

By default, `~mast_table.mast_table.MastTable.show` will display the 
widget in the cell output below the code cell.
