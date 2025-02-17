# Using the Simple MM Controller

We provide a fast way to start using this package when no advanced configuration is needed and you wish to control Motion Matching by script. You can quickly get an animated avatar by using the `SimpleMMController` script.
!!! example
    To explore an example scene, access the Package Manager, select the **Motion Matching** package, and import the **Examples** sample. The sample scene will be located at `Samples/Motion Matching/[version]/Examples/Scenes/00_Basic/ExampleSimpleMMController.unity`.
	
**Getting Started:**

1.  **Import the Samples:** Import the package's example samples through the Package Manager.
2.  **Add the Prefab:** Drag and drop the `Samples/Motion Matching/[version]/Examples/Assets/Prefabs/SimpleMMController` prefab into your scene.

    This prefab contains the following components on its GameObjects:

    * **Root GameObject:** `SimpleMMController` script (the main interface for controlling the character).
    * **Child GameObjects:** Contain a `CharacterController`, a `MotionMatchingController`, and the animated avatar.

3.  **Script Interaction:** Obtain a reference to the `SimpleMMController` script from your custom script.
4.  **Control Velocity:** Use the `#!csharp SetVelocity(Vector2 velocity)` public function to set the desired direction and speed of the avatar's movement. The `Vector2` input represents the velocity in the horizontal plane (X and Z axes).