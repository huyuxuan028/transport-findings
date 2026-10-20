# Furniture Transportation: Findings and Modelling Decisions

This repository records the problem interpretation, research, representation
choice and open decisions. It accompanies independent generator and algorithm
repositories. Vehicle values come directly from the instructor's message.

## What changed

The old homogeneous three-vehicle demonstration is superseded as an initial
research model. The supplied types have different capacities and fuel rates:

| Type | Overall length | Volume | Payload | Fuel L/100km | Top speed |
| --- | --- | --- | --- | --- | --- |
| VW Caddy Cargo SWB | 4.50 m | 3.1 m3 | 680 kg | 4.5-6.2 | 178 km/h |
| Mercedes Sprinter 315 CDI L2H2 | 5.93 m | 10.5 m3 | 1,231 kg | 9.0-9.1 | 161 km/h |
| MAN TGL 12.190 box | 9.20 m | 42.7 m3 | 5,480 kg | 18-24 | 90-110 km/h |

These are adopted course parameters, not a new independent manufacturer audit.
Overall length cannot be substituted for usable cargo length and top speed
cannot be substituted for urban average speed. The project uses five physical
vehicles chosen from these three available types.

## Proposed first problem definition

A static, single-depot, five-vehicle heterogeneous-fleet, multi-trip VRP with
two additive capacity dimensions. Each of the five slots selects one of the
three vehicle types. Vehicles may make sequential trips, returning to depot
between trips. Every indivisible
customer order must be delivered once. There is no daily horizon, time window
or geometric loading check. Orders generated over several transports are made
available together at the start of this simplified optimization batch.

For a trip r using vehicle k, require sum(w_i)<=W_k and sum(v_i)<=V_k. The default
proposed objective is sum_r distance(r) * fuel_rate(k)/100, with distance in km
and fuel rate in litres/100km. Total distance is also selectable. Fuel rates are
fixed instance inputs, so candidates are compared under the same assumptions.

Why propose fuel? With unlimited reuse, no deadline and distance-only cost, the
largest vehicle dominates in capacity: it can reproduce a smaller vehicle's
route at the same distance. Heterogeneity only creates a meaningful choice when
operating cost, availability, deadlines or other restrictions are represented.
Fuel is a supplied parameter, but making it the official objective still needs
supervisor agreement. We do not add unsupported wage, price or CO2 coefficients.
Without a shift limit or fixed vehicle costs, idle vehicles may exist and the
chosen mix cannot be interpreted as a uniquely best operational fleet.

## Representation choice

Use a customer permutation p, customer-indexed allocation a, and a five-entry
vehicle-type vector t. Example: p=[3,1,4,2], a=[0,1,0,2],
t=[Caddy,Sprinter,MAN,Caddy,Sprinter]. Customers 1 and 3 use vehicle 0,
customer 2 uses vehicle 1 and customer 4 uses vehicle 2. Thus the per-vehicle
visit sequences are [3,1], [2] and [4]. Split inserts trip boundaries under
the selected type's capacity.

This makes both order and allocation explicit, as requested. OX preserves the
permutation; uniform allocation crossover preserves customer-indexed choices.
Mutation changes order, vehicle assignment or vehicle type and repairs incompatible
assignments. The decoder
does not mix up customer identity when a permutation position changes.

An alternative is one giant tour with a richer decoder jointly assigning types
and trip boundaries. It reduces chromosome redundancy but makes decoding and
fleet/time coupling harder. Prins [R1] demonstrates that joint tour splitting
and vehicle assignment is a serious research route. Our simpler three-array
approach is an implementation tradeoff, not an assertion of a new algorithm.

## Algorithm-family understanding

| Family | How it searches | Role |
| --- | --- | --- |
| Genetic / memetic search | Population, crossover, mutation, local improvement | Implemented initial approach |
| ILS / variable-neighborhood search | Perturb a route plan and improve neighborhoods | Alternative heterogeneous-fleet research [R4] |
| ALNS | Destroy and repair parts of a solution | Targeted literature extension; not implemented |
| Swarm methods such as ACO / discrete PSO | Construct/recombine discrete routing choices through collective search | Other team's family; needs the same data and validator |
| Exact optimization / DP | Certify optima for a specified small model or decode a fixed order | Fixed-order Split is exact; global search remains heuristic |

No swarm implementation, global-optimum certificate or final winning claim is
included. The original 96.100 km demonstration is a different instance/model
and is not a valid reference objective for the new case.

## Dataset finding

The Kaggle candidate [D1] was downloaded and audited. The 312-row sample has 305
unique ASINs. There are 193 furniture rows with parseable 3D dimensions and a
weight after de-duplication/filtering. A strict requirement for explicit package
dimensions AND shipping/package weight leaves zero usable rows. Source examples
repeat Product Dimensions in the package_dimensions column; item weight may not
be shipping weight. Therefore the initial runnable example explicitly opts into
proxy mode and retains raw strings, source rows and exclusion reasons.

This does not invalidate the instructor's suggestion. It identifies the cleaning
and semantic checks needed before claiming actual furniture-packaging realism.
Product sets, multiple cartons and missing fields still need handling. Obtain
verified packaging records or agree a documented proxy before final benchmarking.

## Decisions to confirm

1. Whether five physical vehicles may be any mix of types or existing inventory limits apply.
2. Static batch versus genuinely dynamic order release.
3. Fuel, distance, duration or a stated multi-objective ranking.
4. Driver/vehicle horizon, loading time and customer time windows.
5. Whether aggregate volume suffices or geometric loading is mandatory.
6. Acceptance of audited catalog proxies versus verified package-only records.
7. Frozen shared benchmark, seed/budget rules and the exact gap reference.

## Planned next refinements

Agree the above decisions; validate more catalog package records; compare different
allocation mutation and local relocation/swap moves; introduce diversity control;
test unseen seeds; then extend the model only as required. Add deadlines before
interpreting schedules as feasible working days. The current example deliberately
demonstrates a simplified initial implementation rather than every rich constraint.

See `REFERENCES.md` for primary research and `MEETING_BRIEF.md` for a short script.
