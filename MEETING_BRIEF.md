# Short Presentation Script

The updated problem is a heterogeneous-fleet routing problem with repeated
transports. The three supplied vehicle types have different payloads, volumes
and fuel consumption, so our earlier identical-vehicle example is only a baseline.

I implemented a generator that loads random furniture, distributes it across
about ten locations, creates a random tour, returns to the depot and starts a
new transport. Bounded normal sampling is used for stop counts, load targets,
synthetic dimensions and weights, coordinates and service times. The included
example has six transports and 64 customers, averaging 10.67 stops per transport.

I also downloaded and audited the suggested Kaggle furniture sample. Some
packaging fields actually repeat product dimensions, and shipping weights are
not consistently available. The prototype explicitly uses labelled proxies;
we should agree whether this is acceptable before the final benchmark.

Our genetic representation has a customer permutation and a vehicle assignment
for each customer. A Split decoder creates feasible trips for each physical
vehicle, and trips run sequentially. This explicitly models both order and
allocation. We researched heterogeneous memetic algorithms and recent multi-trip
genetic-search work to support these choices.

The prototype supports total distance and estimated fuel. I propose fuel for the
first allocation experiments because distance alone gives the largest vehicle
an advantage when trips can be repeated without a deadline. We still need to
confirm the objective, vehicle counts and time horizon.

The findings, generator and algorithms are organized as three separate local Git
repositories, ready to publish. Remote GitHub repositories still need creation
or confirmed destination URLs; no remote publication is claimed.
