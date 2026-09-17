# Week 07 Controls Lab — Responses

Answers and recorded model results from `submission.json`. This document does not recompute or independently validate the results.

## Submission status

- Schema: week07.submission/v1

- Record ID: 56b1fbf0-ba96-427c-996d-b5ef1a5027fa

- Record revision: 373

- Model hash: fnv1a-be327008

- Readiness: Marked incomplete or not ready; missing: claim, reflection, aiUse

## Supplied setup (instructor supplied)

### question — instructor supplied
Calculate required control moment, elevator moment, and the change with airspeed. Explain whether the nominal response meets +0.12 rad/s².

### system — instructor supplied
Illustrative planar pitch model. Aircraft geometry, integration, force conversion and constraints are supplied.

### representation — instructor supplied
Body axes forward/right/down. Positive pitch moment nose-up. Positive Fz downward. Positive elevator trailing edge down. Reference/CG X=0 m; tail X=-3 m.

### inputs — instructor supplied
Iy=5000 kg·m²; target=+0.12 rad/s²; competing=-750 N-m; density=1.225 kg/m³; V=40 m/s; S=16 m²; chord=1.5 m; Cmδ=-0.8/rad; elevator=-5°. Inputs are illustrative, not calibrated.

## Student responses

### physics
**Prompt:** Explain why a downward force aft of the CG gives a positive nose-up moment.

**Student response:**
```
Downward force in the aft means that there is a force that pushes the tail of the aircraft down, resulting in a positive nose-up movement. 
```

### assumptions
**Prompt:** Explain one supplied assumption and what could invalidate it: planar motion, fixed reference, local linear effectiveness, no trim or damping.

**Student response:**
```
Assuming the X-axis goes though the fuselage of the aircraft, out of the nose, the Z-axis goes downwards, towards the ground and the Y-axis protrudes out the the right wing of the aircraft. The rudder and aileron are fixed to ensure there is only rotation about one axis (Y-axis), otherwise known as planar motion. Having either the rudder or aileron be controllable (not fixed) would invalidate the assumption because it allows the aircraft to rotate/move in the other axes. 
```

### model
**Prompt:** Write your demand, dynamic-pressure, coefficient and moment equations. Identify which quantities are supplied and which are unknown.

**Student response:**
```
Demand: Iy*target-competing
Dynamic-Pressure coefficient: q_inf = 0.5*rho*V^2
Cm_delta = (moment_delta)/(q_inf*S*c)
delta_Cm = (partial_Cm/partial_e_deflection)*e_deflection 

Quantities supplied: Iy, target, competing, density(rho), V, S, c, Cm_delta, e_deflection

Quantities unknown: moment_delta, delta_Cm, q_inf
```

### prediction
**Prompt:** Before running your own implementation, predict the sign of its elevator moment and the effect of halving airspeed. Explain the competing moment.

**Student response:**
```
The sign of the elevator moment is negative because the tail is pushed down due to the elevator being deflected up. Halving the airspeed will reduce the elevator moment by a factor of 4, due to the velocity being squared in the q_inf equation. The competing moment is the moment opposing the tail/elevator moment in the Y-axis. 
```

### verification
**Prompt:** Show one independent hand calculation with units. Compare it with your model, and explain a sign, unit, or limiting-case check.

**Student response:**
```
Required control moment: Iy*target-competing

Iy = 5000kgm^2
competing = -750Nm
target = 0.12 rad/s^2

Demand: 5000*0.12-(-750) = 1350Nm



```

### claim
**Prompt:** What do your computed results support at the stated condition? Include a limitation.

**Student response:**
_Missing — no response supplied._

### reflection
**Prompt:** What additional evidence or missing physics would you investigate next?

**Student response:**
_Missing — no response supplied._

### AI use
**Prompt:** Identify the AI tool and how you used it, what you changed, and how you independently checked the result. State “No AI used” if applicable.

**Student response:**
_Missing — no response supplied._

## Equations and model source

The recorded model JSON/expression source follows exactly as supplied. It is not interpreted or recomputed here.

```
{
  "schemaVersion": "week07.student-model/v1",
  "id": "week07-controls-model",
  "version": "1.0.0",
  "slots": [
    {
      "id": "controls.demand",
      "expressions": [
        {
          "name": "requiredMoment",
          "expression": "pitchInertia * requestedAcceleration - competingMoment",
          "unit": "N*m"
        }
      ]
    },
    {
      "id": "controls.effectiveness",
      "expressions": [
        {
          "name": "dynamicPressure",
          "expression": "0.5 * density * airspeed * airspeed",
          "unit": "Pa"
        },
        {
          "name": "deltaCm",
          "expression": "elevatorDerivative * elevatorAngle",
          "unit": "1"
        },
        {
          "name": "deltaMoment",
          "expression": "dynamicPressure * referenceArea * referenceChord * deltaCm",
          "unit": "N*m"
        }
      ]
    }
  ]
}
```

## Recorded verification status

No verification record was supplied.

## Recorded model runs

### Run 1
- Recorded: 2026-09-17T04:12:43.818Z
- Run ID: 490f5418-30a5-4126-bb03-9455eda60e32
- Record revision: 69
- Model hash recorded with run: fnv1a-be327008
- Prediction recorded with run:

```
The sign of the elevator moment is negative because the tail is pushed down due to the elevator being deflected up. Halving the airspeed will reduce the elevator moment by a factor of 4, due to the velocity being squared in the q_inf equation. The competing moment is the moment opposing the tail/elevator moment in the Y-axis. 
```
- Result status: recorded values shown below
- Values: `requiredMoment=1350 N*m`; `dynamicPressure=980 Pa`; `deltaCm=0.06981317007977318 1`; `deltaMoment=1642.0057602762652 N*m`

## Submission instructions

Use Save to GitHub in the app to save both files, commit, and push. Submit your fork URL and the saved commit SHA. Manual fallback: save this file beside `student/submission.json`, run `npm run student:prepare` and `npm run student:validate`, then commit and push student/.
