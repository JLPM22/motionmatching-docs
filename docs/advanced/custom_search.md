# Custom Search

Custom Search is the extension point that lets you combine the classic motion-matching distance with domain logic such as environment-aware penalizations. You implement a subclass of `MotionMatchingSearch` and assign it to your motion matching controller as a ScriptableObject.

!!! warning "Advanced topic"
    This is for advanced users who are comfortable with Burst jobs, data-oriented programming, and the internal layout of features. For environment-aware algorithms and motivation, see the paper: [Environment-aware Motion Matching](https://upc-virvig.github.io/Environment-aware-Motion-Matching/).

## What ships with the package

- `LinearMotionMatchingSearch`: a straightforward full scan over the database.
- `BVHMotionMatchingSearch`: uses BVH buffers to accelerate the scan.
- `MotionMatchingSearch`: the abstract base class you extend.

These are intended as minimal baselines. Environment-aware strategies are provided as examples in the paper’s repository and can be adapted to your needs.

## Lifecycle and responsibilities

A custom search ScriptableObject implements the following methods:

- `Initialize(controller)`: Cache arrays and acceleration structures you will reuse every frame. Typical calls include `FeatureSet.GetBVHBuffers(...)` and `FeatureSet.GetEnvironmentAccelerationStructures(EnvironmentAccelerationConsts, out AdaptativeFeaturesIndices)`.
- `ShouldSearch(controller)`: Return `true` to trigger a new search (e.g., `controller.SearchTimeLeft <= 0`).
- `FindBestFrame(controller, currentDistance)`: Build and run your evaluation (often a Burst job) and return the best frame index.
- `OnSearchCompleted(controller)`: Optional post-search hook, e.g., restore weights or clear debug state.
- `OnUpdateEnvironmentFeatureWeight(controller, environmentFeature, defaultWeight)`: Optional per-frame scaling for an environment feature (anticipation, evasion…). Return the weight you want to use for this frame.
- `DrawGizmos(controller, radius)`: Optional debug visualization.

## Data you’ll use

From `MotionMatchingController` and its `FeatureSet` you can access:

- Normalized feature arrays, tag masks, and validity masks for the classic distance.
- Mean/Std vectors to temporarily denormalize trajectory features for geometric reasoning.
- BVH buffers for early-out pruning in the baseline distance.
- Environment feature acceleration structures via `GetEnvironmentAccelerationStructures(...)` to quickly traverse only relevant candidate regions.

!!! note
    Environment features are stored unnormalized and are not part of the classic distance. You decide how they contribute to the final score.

## Typical patterns with Environment Features

- Distance-to-obstacle penalty: For each candidate, compute the distance between predicted path ellipses and obstacles (points/circles/ellipses) and add a smooth, thresholded penalty.
- Height overlap test: Reject or penalize candidates whose future height range intersects an obstacle’s height interval.

See: [Environment Features](./environment_features.md) for how to author and reason about these inputs.


## References and examples

- Built-in searches: `LinearMotionMatchingSearch`, `BVHMotionMatchingSearch` (in this package).
- Paper and external examples: [Environment-aware Motion Matching](https://upc-virvig.github.io/Environment-aware-Motion-Matching/)
- Authoring extractors: [Custom Trajectory Features](./custom_features.md)
