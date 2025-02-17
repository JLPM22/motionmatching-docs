Character controllers are components that control the character's movement. They are responsible for defining the trajectory of a character to a specific target. The character controller usually receives the input from the user or a path/spline and translates it into trajectories.

!!! warning
	We are currently working on simplifying the definition of new character controllers. We will update the documentation when ready.
	However, feel free to read how ``SpringCharacterController``, ``PathCharacterController`` and ``SplineCharacterController`` components are implemented.
	
!!! tip
	When fine-control is not needed. You can use the ``SimpleMMController`` component to control the character. This component is a wrapper around the ``CharacterController`` and ``MotionMatchingController`` components. It provides a simple interface to control the character's movement. Visit the [Simple MM Controller](using_simple_motion_matching_controller.md) page for more information.