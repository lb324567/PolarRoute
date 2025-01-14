********
Input - Data Loaders
********

Data sources
##############

The polar_route implements a variety of data-loaders to enable working with a 
number of open-source datasets. Links to download some of these datasets and 
there implemented data-loaders are provided in the table below.

+------------------------+------------+---------+----------------------------------------------------------------------------+---------------------+
| Data Type              | Data Name  | Source  | Download                                                                   | Data_loader         |
+========================+============+=========+============================================================================+=====================+
| Sea Ice Concentration  | SIC        | AMSR2   | https://seaice.uni-bremen.de/data/amsr2/asi_daygrid_swath/s6250/netcdf/    | load_amsr_folder    |
+------------------------+------------+---------+----------------------------------------------------------------------------+---------------------+
| Bathymetry             | Elevation  | GEBCO   | https://download.gebco.net/                                                | load_gebco          |
+------------------------+------------+---------+----------------------------------------------------------------------------+---------------------+
| Currents               | uC, vC     | SOSE    | ...                                                                        | load_sose_currents  |
+------------------------+------------+---------+----------------------------------------------------------------------------+---------------------+
|                        |            |         |                                                                            |                     |
+------------------------+------------+---------+----------------------------------------------------------------------------+---------------------+
|                        |            |         |                                                                            |                     |
+------------------------+------------+---------+----------------------------------------------------------------------------+---------------------+



Overview
##############

In this section we discuss functions for loading data into the PolarRoute Mesh.
Data can be added to the Mesh using the *.add_data_points()* function of the Mesh.
This function takes a single argument, a dataframe containing datapoints in a
EPSG:4326 projection. The format in which dataframe must be given is:

+-----+------+------+---------+-----+---------+
| Lat | Long | Time | value_1 | ... | value_n |
+=====+======+======+=========+=====+=========+
| ... | ...  | ...  | ...     | ... | ...     |
+-----+------+------+---------+-----+---------+
|     |      |      |         |     |         |
+-----+------+------+---------+-----+---------+

The data loaders provided collect or create data from heterogeneous data sources
and transform them into the correct format for use by the *.add_data_points()*
function of the Mesh.

Data loader functions located in the file **./polar_route/data_loaders.py** can
be referenced in a configuration file and called in the initialisation of the Mesh.
See the :ref:`Configuration` section of this document for further details. 



.. automodule:: polar_route.data_loaders
    :members:
