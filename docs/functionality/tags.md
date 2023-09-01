# Tags

Tags serve as a robust way to manage unstructured animation data, commonly found in Motion Matching systems. This feature enables you to categorize each animation file using multiple tags and query them in real-time using boolean operations.

## How to Add Tags

1. Navigate to the `MotionMatching/Animation Viewer` from the top menu to open a new scene along with an accompanying window.
2. Reference your *Animation Data* within this window.
3. A stick-like character appears to help you visualize the animation effectively.
4. Click on the `New Tag` button to add a new tag.

<figure markdown>
  ![](../assets/media/animation_viewer.png)
  <figcaption>Animation with three tags and multiple ranges each. The green query tool is set to `walk | idle`. </figcaption>
</figure>


Use double left-click on the timeline to add a new tag range. To delete a range, right-click and select the relevant option. Once you're done, click the `Return` button on the top-left to change to the previous scene.

## Querying Tags at Runtime

### Single Tag Queries

The `MotionMatchingController` component exposes methods to enable or disable tags during the search. To search for a specific tag, use `#!csharp SetQueryTag(string name)`. To disable all tags and search the entire database, use `#!csharp DisableQueryTag()`.

!!! warning
	Minimize calling `#!csharp DisableQueryTag()`, `#!csharp SetQueryTag(string name)` or other query methods every frame to maintain optimal performance.

### Boolean Expression Queries

The system supports complex boolean queries for tags, enabling a variety of operations:

- **Intersection (&)**: Includes poses in both tags (e.g., `happy & walk`).
- **Union (|)**: Includes poses in either of the tags (e.g., `walk | run`).
- **Difference (-)**: Excludes poses in the second tag (e.g., `walk - happy`).
- **Parenthesis**: Alters operation precedence (e.g., `((walk | run) & male) - sad`).

To use boolean queries, utilize the `#!csharp class QueryTag`. Use `#!csharp bool QueryTag.Parse(string expression, out QueryTag query)` for parsing and `#!csharp SetExpression(QueryTag query)` to apply it in the `MotionMatchingController` component. For efficiency, consider parsing and caching the query tags at the beginning of your script.

!!! example
	See the [sample](../samples/tags.md) for an in-depth guide on querying tags.

!!! tip
	The `TagSwitchHelper` component simplifies using boolean queries. Add it to the same GameObject that contains the `CharacterController`. You can alter the query using `#!csharp SetExpression(string expression)` or disable it with `#!csharp DisableQuery()`. Feel free to manipulate the expression directly in the component inspector or through scripting.

Use the preview query tool in the `Animation Viewer` to simulate the result of a complex query.