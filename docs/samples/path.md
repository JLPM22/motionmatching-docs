# Path Character Controller Sample

!!! info
	To access this sample, go to the Package Manager, select the **Motion Matching** package, and then navigate to **Samples > Examples** to click on **Import**.

You can find this sample located at `Examples/Scenes/JLTest/JLScenePathTest.unity`.

This sample differs from the [third person character controller](third_person.md) in that it incorporates three GameObjects—`Test1`, `Test2`, and `Test3`—each containing all the required components to make a character follow a predefined path.

## Key Components

### PathCharacterController

This component allows you to define simple paths that characters will follow within the scene. Similar to the `SpringCharacterController`, it updates query trajectory features based on the path and sends them to the `MotionMatchingController`.

### MotionMatchingController

This is the primary component responsible for executing the Motion Matching algorithm. It should be added to a separate GameObject.

### Joe

This GameObject is the character to be animated. It contains a `MotionMatchingSkinnedMeshRenderer` component that retargets the animations to Unity's built-in animation system.

## How to Play

Press play to see the characters follow the paths based on their target velocities, as specified in the `PathCharacterController`.