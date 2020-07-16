
================
Finding leaflets
================

These notebook demonstrate the performance of several algorithms in
MDAnalysis' LeafletFinder (**on my own personal branch**).

Several methods are available:

* **"center_of_geometry"**: this is the most trivial and the fastest method.
  Lipids are separated into leaflets by their distance to the leaflet
  center-of-geometry. Centers must be passed in as arguments. This is not
  recommended for any leaflet that is not extremely well-defined, and even
  then "graph" or "spectralclustering" is likely more robust.
  It is provided as a cheap way to update leaflets over a trajectory.
* **"graph"**: this works quite well for reasonably dense membranes and
  is the cheapest method outside "center_of_geometry", but relies on the
  cutoff being tuned so that it's big enough to capture neighbouring lipids,
  but small enough to ignore the opposite leaflet. It breaks down as lipids
  get further apart within a leaflet. It works by constructing a NetworkX
  graph of adjacent lipids from a contact matrix, where lipids are defined
  to be in contact if they are within the cutoff. It does not allow users
  to choose how many leaflets they end up with.
* **"spectralclustering"**: this is more expensive than "graph"
  (1.5-2x slower), but somewhat more robust as lipids get further apart.
  It does break down as lipid density decreases. This has the advantage
  that accuracy increases monotonically with cutoff, and being able to
  specify how many leaflets are desired. However, it is not expected to
  work well on multi-membrane systems where the headgroups are very close
  together. This method works by using spectral clustering to group lipid
  headgroups by pairwise distance.
* **"orientation"**: this is by far the most expensive method
  (~5x slower than "graph" right now). However, it is the most flexible.
  It does not significantly degrade in accuracy as lipid density decreases.
  Like "spectralclustering", the accuracy should *mostly* increase monotonically
  with cutoff, and you are able to specify how many leaflets are desired.
  It works by approximating the orientation of each lipid and assuming that
  lipids oriented in the similar directions as their neighbours are in the
  same leaflet.

I (Lily) designed each of these but "graph", if you can call
*gluing things on until it works* "design".

To do (but not soon): optimise "orientation" to make it faster.
It is not written optimally at all.

.. toctree::
   :maxdepth: 1
   
   /leafletfinder/lf_single_bilayer
   /leafletfinder/lf_double_bilayer
   /leafletfinder/lf_vesicle
   /leafletfinder/lf_vesicle_messy