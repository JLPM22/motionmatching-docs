# Environment Features

Environment features let you encode environment/interaction-aware signals alongside your regular trajectory features in the MMData, without changing the classic motion-matching distance. They are authored like trajectory features but are intentionally not normalized and are not part of the traditional search. This design makes them ideal inputs for Custom Search strategies that apply dynamic, context-dependent penalizations or preferences.

!!! warning "Advanced topic"
    This section targets advanced users with deep knowledge of Motion Matching and data authoring. We strongly recommend reading the paper: [Environment-aware Motion Matching](https://upc-virvig.github.io/Environment-aware-Motion-Matching/).

## Why environment features?

Traditional motion matching minimizes a distance over normalized trajectory and pose features. Many environment cues (obstacles, crowds, traffic rules, terrain affordances…) don’t fit well into that normalized distance. By storing these cues as environment features:

- They keep their natural, unbounded units (not normalized), so you can reason in meters, radii, heights, densities, etc.
- They are ignored by the default search, so you won’t skew the core distance metric.
- You can consume them in a Custom Search to compute dynamic penalizations (e.g., distance to obstacles along the predicted path) and blend classical similarity with environment awareness.

Examples and full algorithms are described in the paper and showcased in the external repository that builds on this package: [Environment-aware Motion Matching](https://upc-virvig.github.io/Environment-aware-Motion-Matching/)

## Authoring: same flow as trajectory features

You define environment features in the MMData using the same extractor pattern as trajectory features.

1. Implement a feature extractor (see Custom Trajectory Features) that outputs the signal you need (e.g., ellipse parameters around the future path, future height window, crowd descriptors…).
2. Add it to your `MotionMatchingData` asset under Environment Features.
3. Generate the database.

Key guidelines:

- Keep values in the character’s local space, like any other feature in the system.
- Do not normalize environment features. They are stored “as is” to preserve semantics for your search.
- Prefer compact encodings (e.g., 2D/4D tuples rather than large grids) to keep the database size reasonable.

!!! tip
	If you haven’t created custom extractors before, start here: [Custom Trajectory Features](./custom_features.md). The same extractor base classes and ScriptableObject workflow apply.

## How they are used at runtime

Environment features are available to the Motion Matching controller but are not included in the default distance calculation. Custom searches can read them to modulate the candidate score. Typical patterns include:

- Obstacle avoidance: compute a penalty from the distance between predicted path ellipses and environment obstacles (points, circles, ellipses).
- Height-aware navigation: discard or penalize candidates whose future height range overlaps with obstacles or impassable terrain.
- Interaction anticipation: scale penalties by target speed or intent to be more conservative at higher velocities.

Under the hood (advanced):

- You can retrieve acceleration structures for environment features via `FeatureSet.GetEnvironmentAccelerationStructures(EnvironmentAccelerationConsts, out AdaptativeFeaturesIndices)` to scan candidates efficiently.
- Use classic BVH buffers (`GetBVHBuffers`) if you also want early-out culling on standard trajectory features.
- You’ll often combine normalized trajectory features (denormalized when needed using `Mean`/`Std`) with raw environment features in a single score.

## Feature vector layout and offsets

Environment features are stored inside each feature vector after the classic features. The order and offsets are fixed at database build time:

- Trajectory features (normalized)
- Pose features (normalized)
- Environment features (raw, not normalized)

Key fields exposed by `FeatureSet`:

- `FeatureSize`: total floats per feature vector (trajectory + pose + environment)
- `FeatureStaticSize`: floats excluding environment features (trajectory + pose)
- `TrajectoryOffset[i]`: start offset of trajectory feature i within a vector
- `PoseOffset`: start offset of the pose block within a vector
- `EnvironmentOffset[i]`: start offset of environment feature i within a vector
- `NumberPredictions*` and `NumberFloats*`: per-feature counts to step predictions and dimensions

Schematic layout per candidate k:

```
Features[k * FeatureSize + 0 .. k * FeatureSize + PoseOffset)                 -> Trajectory block
Features[k * FeatureSize + PoseOffset .. k * FeatureSize + FeatureStaticSize) -> Pose block
Features[k * FeatureSize + FeatureStaticSize .. (k + 1) * FeatureSize)        -> Environment block
```

You can either compute indices manually in Burst jobs or use the helpers provided by `FeatureSet`:

- `Get1D/2D/3D/4DEnvironmentFeature(k, i, p)`
- `Get1D/2D/3D/4DTrajectoryFeature(k, i, p, denormalize: bool)`
- `GetPoseFeature(k, j, denormalize: bool)`

!!! note
	Environment features are not part of `Mean`/`StandardDeviation`. If you temporarily need world-scale values for trajectory/pose during scoring, either call `DenormalizeFeatureVector` on a copy of the query/candidate, or use `Mean`/`StandardDeviation` to undo normalization per component. Leave environment features untouched.

!!! tip
	Features with multiple prediction horizons are laid out contiguously per feature: all components of prediction 0, then prediction 1, etc. This applies equally to trajectory and environment features.

## Pairing with a Custom Search

Environment features shine when paired with a Custom Search that implements the domain logic for penalization. See the dedicated page: [Custom Search](./custom_search.md).

At a high level, a custom search will:

1. Decide when to search (e.g., `controller.SearchTimeLeft <= 0`).
2. Build and run a job that evaluates the motion database using both the classic distance and any environment penalties.
3. Return the best frame index.
4. Optionally provide gizmo/debug drawing and per-frame weight tweaks (anticipation, evasion, etc.).

## Example use cases (from the paper)

- Crowd/obstacle-aware motion matching: penalize candidates whose predicted path intersects nearby circles/ellipses around obstacles or agents, with a thresholded distance function and Burst-accelerated evaluation.
- Height-aware search: reject candidates whose future height interval overlaps with obstacle height ranges.

See the paper and external examples for full details and videos: [Environment-aware Motion Matching](https://upc-virvig.github.io/Environment-aware-Motion-Matching/)

## Related reading

- Paper: Environment-aware Motion Matching — [Environment-aware Motion Matching](https://upc-virvig.github.io/Environment-aware-Motion-Matching/)
- Custom extractors: [Custom Trajectory Features](./custom_features.md)
- Implementing a search: [Custom Search](./custom_search.md)

