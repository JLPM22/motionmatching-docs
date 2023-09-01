# Unity Animator Integration Sample

!!! info
	To import this sample, navigate to the Package Manager, select **Motion Matching**, and then go to **Samples > Examples** and click on **Import**.

You can find this sample at `Examples/Scenes/JLTest/JLSceneUnityAnimationIntegration.unity`. This sample serves as an extension of the [third person character controller](third_person.md) by integrating it with Unity's `Animator` component.

## Key Changes

- The `Animator` component in the `Joe` GameObject is configured with an animator controller that includes a single animation clip.
- The `MotionMatchingSkinnedMeshRenderer` component in `Joe` specifies a custom Avatar Mask. This mask informs the Motion Matching system that only the joints in the lower body should be animated.

## How to Play

Press play to see how the `Animator` component animates the upper body of the character, while Motion Matching takes care of the lower body and locomotion. You can also remove the reference to the Avatar Mask to see how the system automatically blends into a full-body Motion Matching pose.

!!! note
	For a comprehensive understanding of how Unity's animation system and Motion Matching are integrated, please refer to [this section](../functionality/unity_animator.md).