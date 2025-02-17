# Tags Sample

!!! info
	To import the sample, go to the Package Manager, select **Motion Matching**, and then navigate to **Samples > Examples**. Click on **Import**.

This sample is located at `Examples/Scenes/01_Advanced/ExampleTags.unity`. It builds upon the [third person character controller](third_person.md) example, incorporating the `TagSwitchHelper` component to demonstrate the usage of tags in Motion Matching.

## Key Changes

- The `TagSwitchHelper` component is attached to the `CharacterController`. This component allows the user to input both simple and complex tag queries. By default, the query expression is set to `walk`.

## How to Play

Press the play button to start the scene. Initially, the character will only be able to walk. To toggle the tag query on or off, press **T** on the keyboard or **A** on an Xbox controller. When the query is disabled, the character will be free to utilize the full range of animations in the database, allowing it to walk or run. To restrict the character to an idle state, change the query expression to `idle`. If query expressions are not enabled, press **T** to enable them, and the character will not be able to move

!!! note
	For a comprehensive guide on how tags work in conjunction with Motion Matching, please refer to [this section](../../advanced/tags.md).