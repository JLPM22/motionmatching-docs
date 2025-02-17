# Quick Start Guide

Follow these steps to get started with the Motion Matching package for Unity.

## Installation Steps

1. Ensure you have **Unity 6+** installed (untested on other versions).

2. Open the Unity Editor and navigate to **Window > Package Manager**.

3. In the Package Manager, click **Add (+) > Add package by git URL...**.

4. Insert the following URL into the git URL field and click **Add**:
	```
	https://github.com/JLPM22/MotionMatching.git?path=/com.jlpm.motionmatching
	```

	!!! warning inline end
	    Note: All sample scenes use the Universal Render Pipeline (URP). Conversion may be necessary if you are using a different render pipeline.

5. *[Optional]* In the Package Manager, click on **Motion Matching**, then import the example scenes by selecting **Samples > Examples > Import**.

6. *[Optional]* Go to ``Samples/Motion Matching/[version]/Examples/Scenes/`` in the Project Window to explore the sample scenes.

## Project Overview

### Directories

- `Samples/Animations`: Contains motion capture (MoCap) files (with *.txt* extensions but originally *.bvh* files) and *MMData* files to define the animation database for the Motion Matching System.
  
- `StreamingAssets/MMDatabases`: Contains the processed pose and feature databases, as well as skeletal information. This directory is automatically created when generating databases from an *MMData* file.

### Key Components

Demo scenes consist of two primary GameObjects:

1. **Character Controller**: Creates trajectories and imposes positional constraints, like limiting the maximum distance between the simulated and animated character positions.

2. **MotionMatchingController**: Handles all Motion Matching operations. It provides adjustable parameters for enabling/disabling features like inertialize blending or foot locking.

Feel free to tweak and explore these components to get a better understanding of the system.