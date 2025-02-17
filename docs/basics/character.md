# Animating a Character with Motion Matching

## Overview

Animating a character using Motion Matching involves coordinating between the Character Controller and the Motion Matching controller. While the Character Controller takes user input to generate trajectories and features, the Motion Matching controller manages the databases, motion matching search, and pose post-processing. Lastly, the system retargets the calculated skeleton to Unity's rendering system, as this project does not use Unity's native animation system for Motion Matching.

## Steps for Animation

### Create Animation Database

- First, generate an animation database (*MMData*) by following the [previous section](animation_database.md) on creating *MMData*.

	!!! tip
		You can use the demo scene at ``Scenes/00_Basic/ExampleSimpleMMController.unity`` as a template.

### Add Character Controller

- Create an empty GameObject and attach a Character Controller component to it. Make sure to reference the ``MotionMatchingController`` (to be created in step 3) in the Motion Matching field. Keep the feature names unchanged unless you've also modified them in your custom *MMData*.

### Add Motion Matching Controller

- Create another empty GameObject and attach a ``MotionMatchingController`` component to it. Reference the Character Controller from step 2 and the *MMData* file you've prepared.

### Add Avatar and Renderer

- Insert an avatar into the scene. As a reference, you can use ``Assets/Models/Joe/Joe.prefab``, which should be imported as a humanoid. Attach a ``MotionMatchingSkinnedMeshRenderer`` component to the avatar and reference the ``MotionMatchingController`` from step 3.

### Test Animation

- Click on the Play button in Unity to see your animated character in action!

By following these steps, you should now have a functioning Motion Matching animated character.
