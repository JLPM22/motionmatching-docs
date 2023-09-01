# Third Person Character Controller Sample

!!! info
	To access this sample, first open the Package Manager, select **Motion Matching**, and then import the examples by navigating to **Samples > Examples** and clicking **Import**.

The sample can be located at `Examples/Scenes/JLTest/JLSceneThirdPerson.unity`. This sample showcases human locomotion animated using the Motion Matching system. It is designed to work with Unity's new action-based Input System and is compatible with both keyboard and Xbox controllers.

## Key Components

### CharacterController

!!! info inline end
	For more information about spring-based character controllers, refer to [this article](https://theorangeduck.com/page/spring-roll-call) by Daniel Holden.

This component manages user input and utilizes the `SpringCharacterController` to predict the character's movement. It is responsible for generating the query trajectory features that are passed to the `MotionMatchingController`.

??? tip
	If you find that Motion Matching isn't precisely following the Character Controller's trajectory, consider enabling the `Do Clamping` option within the `SpringCharacterController`. This will enforce a maximum deviation limit but may result in some foot sliding.

### MotionMatchingController

This component performs the actual Motion Matching and should be placed in a separate GameObject.

### Joe

This GameObject represents the character you want to animate. It includes a `MotionMatchingSkinnedMeshRenderer` component for retargeting to Unity's animation system.

## How to Play

After pressing play, use the **WASD** keys or the **left joystick** on your Xbox controller to move the character. We recommend using an Xbox controller to fully experience the capabilities of Motion Matching, which can adapt to varying speeds and rotational angles.

Press **F** on the keyboard or the **Y** button on your Xbox controller to lock the character's orientation.

When Gizmos are enabled, the position of the Character Controller will appear in orange, and the predicted future positions will be displayed in light purple.