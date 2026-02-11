# Project

In this project,
we seek to optimize an aircraft of approximately 150 passengers 
that flies at Mach 0.78, and uses liquid hydrogen.

## The OAD problem under uncertainty

In presence of uncertainty sources,
the OAD problem is 

- finding the values of the design parameters 
- that minimize the **mean** of the maximum take-off mass (MTOM, `mtom` in the code) 
- under operation constraints expressed as **margins with a factor of 2**.

## The design parameters

The **design parameters** are :

- the maximum sea level static thrust  (100 kN ≤ slst ≤ 200 kN, default: 150 kN),
- the number of passengers  (120 ≤ n_pax ≤ 180, default: 150),
- the wing area (100 m² ≤ area ≤ 200 m², default: 180 m²),
- the wing aspect ratio  (5 ≤ ar ≤ 20, default: 9).

## The operational constraints

The **operational constraints** are :

- the take-off field length (tofl ≤ 1900 m),
- the approach speed (vapp ≤ 135 kt),
- the vertical speed (300 ft/min ≤ vz),
- the wing span (span ≤ 40 m),
- the wing length (length ≤ 45 m),
- the fuel margin (0% ≤ fm).

## The uncertain parameters

These uncertain parameters are modelled as random variables defined by probability distributions:

| Variable | Distribution          |
|----------|-----------------------|
| `gi`     | T(0.35, 0.4, 0.405)   |
| `vi`     | T(0.755, 0.800, 0805) |
| `aef`    | T(0.99, 1., 1.03)     |
| `cef`    | T(0.99, 1., 1.03)     |
| `sef`    | T(0.99, 1., 1.03)     |

where `T(minimum, mode, maximum)` represents the [triangular distribution](https://en.wikipedia.org/wiki/Triangular_distribution).

The parameters `aef`, `cef` and `sef` are related
to the three main technical areas involved in aircraft design,
namely aerodynamics, propulsion and structure.
The lower, the better.
These factors are representing the unknown
included in any creative activity.
Their probability distributions are not symmetrical
as it is always easier to make something less efficient than expected...

## TODO

### A. Uncertainty quantitication

Propagate the uncertainties through the multidisciplinary system
and quantify their impact on the outputs of interest, namely objective and constraints.

1. using a maximum of 100 evaluations of the multidisciplinary system
2. using a maximum of 500 evaluations of the multidisciplinary system
3. using a maximum of 1000 evaluations of the multidisciplinary system

!!! tip

    You can do whatever you want with your maximum number of evaluations, including building a surrogate model.

!!! warning

    It would be appropriate to conduct this study twice:

    1. setting $x$ to its initial value $x^{(0)}$,
    2. setting $x$ to its optimal value $x^*$ found with the MDO problem without uncertainties. 

### B. Sensitivity analysis

Propagate the uncertainties through the multidisciplinary system
and analyze the sensitivity of the outputs of interest, namely objective and constraints.

1. using a maximum of 100 evaluations of the multidisciplinary system
2. using a maximum of 500 evaluations of the multidisciplinary system
3. using a maximum of 1000 evaluations of the multidisciplinary system

!!! tip

    You can do whatever you want with your maximum number of evaluations, including building a surrogate model.

!!! warning

    It would be appropriate to conduct this study twice:

    1. setting $x$ to its initial value $x^{(0)}$,
    2. setting $x$ to its optimal value $x^*$ found with the MDO problem without uncertainties.

### C. MDO under uncertainty

1. Solve the OAD problem using the MDF formulation and a statistic estimation technique.
2. Solve the OAD problem using the MDF formulation and another statistic estimation technique.
3. Analyze the results, compare the solution obtained with 1, 2 and uncertainty-free MDO.

!!! warning

    It would be appropriate to conduct this study twice:

    1. starting from the initial value $x^{(0)}$,
    2. starting from the initial value $x^*$, which is the optimum found with the MDO problem without uncertainties.

### Tips

#### IDF

```python
from gemseo.settings.formulations import IDF_Settings

IDF_Settings(include_weak_coupling_targets=True)
```

#### Update default input values

```python
from gemseo.utils.discipline import update_default_input_values

update_default_input_values(disciplines, {input_name: input_value, ...})
```
