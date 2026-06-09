# Bug / Compatibility Report: ONS with MES on RSS-scale Torch Dedicated Server

## Summary

Orbital NPC Spawns successfully intercepted a MES NPC spawn on a Torch dedicated server running an RSS-scale world. The initial velocity correction worked, but the corrected NPC lost speed quickly after release. MES also logged its own warning about spawning NPC grids beyond 6500 km from world center.

This may be expected behavior or a world-scale limitation rather than a hard ONS bug, but the trace should be useful for dedicated-server/RSS/MES compatibility testing.

## Environment

- Server: Torch dedicated server
- Space Engineers: 1.209.24
- ONS workshop ID: 3738633660
- MES workshop ID: 1521905890
- World type: Real Solar Systems / large-scale RSS setup
- View distance in ONS trace: 8000 m
- ONS diagnostic command used: `/ons diag all`
- Player speed during intercept: about 608 m/s
- Dominant body detected by ONS: Queres
- Gravity profile in trace: InverseSquare, `ro=True`

## Reproduction steps

1. Start the Torch dedicated server with ONS, MES, Real Solar Systems, and the attached mod list.
2. Enable ONS diagnostics with `/ons diag all`.
3. Fly near a planet in the RSS system at high speed, above 200 m/s.
4. Wait for MES to spawn a dynamic NPC grid.
5. Observe the ONS trace output.

## Observed result

ONS detected MES and reported the MES API as ready:

```text
mesLoaded=True mesDetail=api ready; mod list hint=workshop id=1521905890.sbm friendly=Modular Encounters Systems publishedId=1521905890
```

ONS intercepted a MES NPC grid:

```text
[Intercept] grid=(NPC-PLX) Shadow ... refSpeed=608.2 case=StableOrbit applied=True unknownSignal=False
```

The grid was corrected from zero velocity to a pass-by/orbit-like trajectory:

```text
[Kinematics] strategy=StableOrbit/PassBy v_before=0,0,0 v_after=-578,4,97 ... dampeners=off hostile=True enforce=180t
```

Velocity enforcement completed successfully:

```text
[Kinematics] enforce-done ... target=587 actual=587 released
```

The trajectory was classified as stable:

```text
[Orbit] subject=(NPC-PLX) Shadow planet=Queres ... speed=586.6 class=near-circular stable=True e=0.048 ... period=1226s
```

## Follow-up behavior

After release, the grid slowed down rapidly:

```text
speed=535
speed=415
speed=298
speed=191
speed=121
speed=88
speed=54
```

The grid was eventually dropped from diagnostics after passing beyond 2x view distance:

```text
watch-dropped-beyond-2x-view ... dist=16001 limit=16000 view=8000 stillPresent=true
```

## Related MES warning

MES logged the following warning during the same run:

```text
MES Spawner / SpawnRecord: WARNING: Spawning NPC Grids beyond 6500km from world center may result in loss of precision when grids are placed in the world or when they utilize path finding. This is because of game engine limitations.
```

The test world is an RSS-scale world. The intercepted grid position in the ONS trace was roughly 38,731 km from world center.

## Expected / desired behavior

For RSS/Real Orbits use cases, it would be ideal if corrected MES NPCs could preserve a believable flyby/orbital trajectory for longer after the initial intercept, or if the trace could clearly mark when later braking is expected because normal AI has taken over.

## Attachments

Files included with this report package:

- `logs/full-sanitized/ONS_Diagnostic_Trace_20260609_144556.txt`
- `logs/full-sanitized/Keen-2026-06-09.log`
- `logs/full-sanitized/Torch-2026-06-09.log`
- `logs/full-sanitized/Chat_ONS_Commands_Only.log`
- `evidence/key-extracts/Keen_ONS_MES_Key_Extract.txt`
- `evidence/key-extracts/MOD_ERROR_Summary.txt`

## Sanitization note

This public package intentionally excludes raw private server files, save files, credentials, IP addresses, local machine paths, Steam IDs, and unrelated chat. Paths and sensitive values in logs are redacted with placeholders such as `<SAVE_PATH>`, `<TORCH_INSTANCE>`, `<IP_ADDRESS>`, and `<SERVER_NAME>`.
