# Short Presentation Script

The updated problem is a five-vehicle heterogeneous-fleet routing problem with repeated
transports. We choose the five vehicles from three types with different payloads, volumes
and fuel consumption, so our earlier identical-vehicle example is only a baseline.

I implemented a generator that loads random furniture, distributes it across
about ten locations, creates a random tour, returns to the depot and starts a
new transport. Bounded normal sampling is used for stop counts, load targets,
synthetic dimensions and weights, coordinates and service times. The included
example has ten transports and 102 customers, averaging 10.2 stops per transport.

I also downloaded and audited the suggested Kaggle furniture sample. Some
packaging fields actually repeat product dimensions, and shipping weights are
not consistently available. The prototype explicitly uses labelled proxies;
we should agree whether this is acceptable before the final benchmark.

Our genetic representation has a customer permutation, an assignment for each
customer and a type for each of five physical vehicles. A Split decoder creates feasible trips for each physical
vehicle, and trips run sequentially. This explicitly models both order and
allocation. We researched heterogeneous memetic algorithms and recent multi-trip
genetic-search work to support these choices.

The prototype supports total distance and estimated fuel. I propose fuel for the
first allocation experiments because distance alone gives the largest vehicle
an advantage when trips can be repeated without a deadline. We still need to
confirm the objective, vehicle inventory limits and time horizon.

The findings, generator and algorithms have three public GitHub repositories.
The five-vehicle files are ready to replace the older uploaded version.
