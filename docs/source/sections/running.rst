********
Runnning Codebase
********

The codebase current can be run from either pre-defined python functions or via a command line interface. Outlined below is how to run the separate sections of the software package using either of the two methods.


Command Line Interface
##############

^^^^^^^^^^^^^^^^^^
Meshing
^^^^^^^^^^^^^^^^^^
Once installed, a mesh can be created from a config file using the command:
::
    create_mesh <config.json>

optional arguments are
::
    -v (verbose logging)
    -o <output location> (set output location for mesh)


^^^^^^^^^^^^^^^^^^
Vehicle Specific Information
^^^^^^^^^^^^^^^^^^
Vehicle specific information can be encoded into the mesh using
the command:
::
    add_vehicle <mesh.json>

optional arguments are
::
    -v (verbose logging)
    -o <output location> (set output location for mesh)



^^^^^^^^^^^^^^^^^^
Route Planning
^^^^^^^^^^^^^^^^^^
Optimal routes through a mesh can be calculated using the command:
::
    optimise_routes <vessel_mesh.json> <waypoints.csv>

optional arguments are
::
    -v (verbose logging)
    -o <output location> (set output location for mesh)
    -p (output only the caculated path, not the entire mesh


^^^^^^^^^^^^^^^^^^
Plotting
^^^^^^^^^^^^^^^^^^
Meshes produced at any stage in the route planning process can be visualised using the GeoPlot library found at `Link <https://github.com/antarctica/GeoPlot>` 



Python & iPython Notebooks
##############

^^^^^^^^^^^^^^^^^^
Meshing
^^^^^^^^^^^^^^^^^^

The configuration file is loaded into the codebase. As this file will be appended to throughout the construction of the paths we name the output file `info`. More information can be found in the configuration section of the manual for the construction of this input file.
::
    import json
    with open('config.json', 'r') as f:
        info = json.load(f)    

The `info` object is passed to the polar_route software to construct a mesh. Once the mesh is constructed we output the json object, `mesh`. More information of the output from this section can be found in the outputs section of the manual pages.
::

   from polar_route.mesh import Mesh
   cg = Mesh(info)
   mesh = cg.to_json()


This file can be saved or passed, given below, or passed as an active object:
::
    with open('mesh.json') as f:
        json.dumps(mesh)


It the file was saved then the object can be loaded using:
::
    import json
    with open('mesh.json', 'r') as f:
        mesh = json.load(f)    

^^^^^^^^^^^^^^^^^^
Vehicle Specific Information
^^^^^^^^^^^^^^^^^^
The `mesh` object now with mesh information is used as an input by the vessel performance class. The section of the codebase alters the neighbour graph and appends new derived quantities to the mesh information. This altered object is then output to `info`.
::
   from polar_route.vessel_performance import VesselPerformance
   vp = VesselPerformance(mesh)
   vessel_performance = vp.to_json()

^^^^^^^^^^^^^^^^^^
Route Optimisation
^^^^^^^^^^^^^^^^^^
Now that the vessel dependent environmental mesh is defined, and represented in the `vessel_performance` object, we can construct routes, with parameters defined by the user in the configuration file. Waypoints are passed as an input file path, `waypoints.csv`, discussed more in the Inputs section of the manual pages.  The route construction is done in two stages: construction of the meshed dijkstra optimal routes, `.compute_routes()`; and, the smoothing of the dijkstra routes to further optimise the solution and reduce mesh dependencies, `.compute_smooth_routes()`. During `.compute_routes()` the paths are appended to the object as an entry `paths`, which are replaced by the smoothed paths after running `.compute_smooth_routes()`. An additional entry `waypoints` is generated to give the waypoints information used in route construction. For further info about the structure of the outputs of the paths please see the Outputs section of the manual.
::
    from polar_route.route_planner import RoutePlanner
    rp = RoutePlanner(vessel_performance,'waypoints.csv')
    rp.compute_routes()
    rp.compute_smoothed_routes()
    info = rp.to_json()



