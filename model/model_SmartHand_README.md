# model_SmartHand README

This file documents the use of the MEX function model_SmartHand.

The actual MEX functions are model_SmartHand.mexw32 and model_SmartHand.mexw64, for 32 and 64-bit Matlab respectively.

The MEX function has several ways in which it can be used:


### Initialization
```
load('model_params');
model_SmartHand('Initialize',model);
```

This has to be the very first call of the MEX function.  During initialization, the model will be initialized, and various model parameters will be read structure model.


### Dynamics
```
[f, dfdx, dfdxdot, dfdu] = model_SmartHand('Dynamics',x, xdot, u, extF);
```

This is to evaluate the model dynamics in the implicit form f(x, xdot, u) = 0.

| Inputs | Size | Description |
|--------|------|-------------|
| x | nstates x 1 | Model state, consists of ndof generalized coordinates (rad), ndof generalized velocities (rad/s), nmus CE lengths (relative to LCEopt), and nmus muscle active states (between 0 and 1)|
| xdot | nstates x 1 | State derivatives |
| u | nmus x 1 | Muscle excitations |

| Optional input | Size | Description |
|----------------|------| ------------|
| extF | 3 x 5 | External forces (x-y-z) applied to the 5 finger tips |

| Output | Size | Description |
|--------|------| ------------|
| f | nstates x 1 | Dynamics imbalance |

| Optional outputs | Size | Description |
|------------------|------| ------------|
| dfdx | nstates x nstates sparse | Jacobian of f with respect to x |
| dfdxdot | nstates x nstates sparse | Jacobian of f with respect to xdot |
| dfdu | nstates x nmus sparse | Jacobian of f with respect to u |

Note: the three Jacobians must always be requested together, or not at all.


### Extract optimal fiber lengths (LCEopt) of the muscle elements
```
LCEopt = model_SmartHand('LCEopt');
```
| Output | Size | Description |
|--------|------| ------------|
| Lceopt | nmus x 1 | Optimal fiber length parameters for all muscle elements (in meters) |


### Extract tendon slack lengths (SEEslack) of the muscle elements
```
SEEslack = model_SmartHand('SEEslack');
```

| Output | Size | Description |
|--------|------| ------------|
| SEEslack | nmus x 1 |	Slack length of series elastic element for all muscle elements (in meters) |


### Extract limits of all degrees of freedom
```
limits = model_SmartHand('Limits');
```

| Output | Size | Description |
|--------|------| ------------|
| limits | 2 x ndof | Lower and upper limits of the ndof dofs, in radians |


### Extract position and orientation of the bones for visualization
```
vis = model_SmartHand('Visualization', x);
```
| Input | Size | Description |
|-------|------| ------------|
|	x	| nstates x 1 | Model state |

| Output | Size | Description |
|--------|------| ------------|
| stick | nSegments x 12 | Position (3) and orientation (3x3) of nSegments segments |

Note: Output only depends on the first ndof elements of x (the joint angles)


### Compute joint moments
```
moments = model_SmartHand('Jointmoments', x);
```

| Input | Size | Description |
|-------|------| ------------|
|	x	| nstates x 1 | Model state |

| Output | Size | Description |
|--------|------| ------------|
| moments | ndof x 1 | Joint moments (N m) |


### Compute muscle forces
```
forces = model_SmartHand('Muscleforces', x);
```

| Input | Size | Description |
|-------|------| ------------|
|	x	| nstates x 1 | Model state |

| Output | Size | Description |
|--------|------| ------------|
| forces | nmus x 1 | Muscle forces (N) |


### Compute moment arms
```
momentarms = model_SmartHand('Momentarms', x);
```
| Input | Size | Description |
|-------|------| ------------|
|	x	| nstates x 1 | Model state |

| Output | Size | Description |
|--------|------| ------------|
| momentarms | nmus x ndof sparse | Moment arms of nmus muscle elements at ndof joints (meters) |


### Compute muscle-tendon lengths
```
lengths = model_SmartHand('Musclelengths', x);
```
| Input | Size | Description |
|-------|------| ------------|
|	x	| nstates x 1 | Model state |

| Output | Size | Description |
|--------|------| ------------|
| lengths | nmus x 1 | Length of nmus muscle elements (meters) |

Note: Output only depends on the first ndof elements of x (the joint angles)


### Find muscle name
```
name = model_SmartHand('Musclename', number);
```

| Input | Size | Description |
|-------|------| ------------|
| number | scalar |	Number of a muscle element, must be in the range 1..nmus |

| Output | Size | Description |
|--------|------| ------------|
| name | string |	Name of the muscle element, as defined in the model structure |
