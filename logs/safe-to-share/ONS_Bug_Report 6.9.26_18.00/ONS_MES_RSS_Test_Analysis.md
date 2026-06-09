# ONS + MES + RSS Test Analysis

## Result

The test produced a valid ONS diagnostic trace. ONS did start on the dedicated Torch server and it did intercept at least one MES NPC spawn.

The successful intercept is:

- Grid: `(NPC-PLX) Shadow`
- Source context: MES was detected and the MES API was ready
- Player reference speed: `608.2 m/s`
- Case: `StableOrbit`
- Strategy: `StableOrbit/PassBy`
- Applied: `true`
- Gravity profile: `InverseSquare`
- Real Orbits/RSS-style gravity state in trace: `ro=True`
- Dominant body: `Queres`
- View distance in trace: `8000 m`

The most important line is:

```text
[Intercept] grid=(NPC-PLX) Shadow ... refSpeed=608.2 case=StableOrbit applied=True unknownSignal=False
```

## What ONS did

ONS changed the NPC grid from a stationary spawn to a moving flyby/orbit-like pass:

```text
v_before=0,0,0
v_after=-578,4,97
dampeners=off
enforce=180t
```

After the short enforcement period, the trace confirms the velocity target was reached:

```text
enforce-done ... target=587 actual=587 released
```

It also classified the resulting trajectory as stable:

```text
class=near-circular stable=True e=0.048 peri=113293 apo=124780 period=1226s
```

## What was skipped

ONS correctly skipped several grids before the successful intercept:

- `Economy_Outpost_9` skipped because it was a static grid.
- Two `(ACI) Xeos Superiority Fighter (SF)` grids were skipped because they were not NPC owned.
- `Celes Mk2 typeS (vanilla)` was deferred because it had no owners yet.

These skips look expected for ONS, not broken behavior.

## Important MES/RSS warning

MES logged this warning during the same run:

```text
MES Spawner / SpawnRecord: WARNING: Spawning NPC Grids beyond 6500km from world center may result in loss of precision when grids are placed in the world or when they utilize path finding. This is because of game engine limitations.
```

The successful ONS intercept happened at roughly 38,731 km from world center based on the trace spawn position:

```text
p_after=4303188,29238103,25034026
approx distance from origin = 38,731 km
```

So MES did spawn something at that scale, but MES also warned that this is outside the engine-safe precision range.

## Possible issue / limitation

After ONS released control and the grid moved beyond view distance, the NPC speed decayed quickly:

```text
14:53:54 speed=535 m/s
14:53:59 speed=415 m/s
14:54:04 speed=298 m/s
14:54:09 speed=191 m/s
14:54:14 speed=121 m/s
14:54:19 speed=88 m/s
14:54:24 speed=54 m/s
```

This may be expected because the mod description says normal AI takes over later. It is still worth reporting as feedback: the initial high-speed intercept works, but the NPC loses orbital/flyby velocity after release.

## Conclusion

ONS is not just passively loaded anymore. In this run it demonstrably worked on a MES NPC spawn.

No ONS crash was found in the provided logs. The main caveat is world scale: MES itself warns about spawning beyond 6500 km from world center, and this test occurred far beyond that.
