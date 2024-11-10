---
tags:
---

[[Algorithms]]

Source: [Maxim Gumin's GitHub](https://github.com/mxgmn/WaveFunctionCollapse)

## Overview
---
Wave function collapse (WFC) is an algorithm that generates output patterns that are locally similar to a given input pattern.

![[wfc.gif]]

Local similarity means that:
* (C1) The output should contain only those NxN patterns of pixels that are present in the input.
* (Weak C2) Distribution of NxN patterns in the input should be similar to the distribution of NxN patterns over a sufficiently large number of outputs. In other words, probability to meet a particular pattern in the output should be close to the density of such patterns in the input.

In the examples a typical value of N is 3.

![[patterns.png]]

The above examples use bitmaps for input. WFC initializes an output bitmap in a completely unobserved state, where each pixel value is in superposition of colors of the input bitmap (so if the input was black & white then the unobserved states are shown in different shades of grey). The coefficients in these superpositions are real numbers. Then the program goes into the observation-propagation cycle:

* On each observation step an NxN region is chosen among the unobserved which has the lowest Shannon entropy. This region's state then collapses into a definite state according to its coefficients and the distribution of NxN patterns in the input.
* On each propagation step new information gained from the collapse on the previous step propagates through the output.

On each step the number of non-zero coefficients decreases and in the end we have a completely observed state, the wave function has collapsed. This process is not truly quantum because it uses real numbers instead of imaginary numbers. It is, however, inspired by quantum mechanics.

It may happen that during propagation all the coefficients for a certain pixel become zero. That means that the algorithm has run into a contradiction and can not continue. The problem of determining whether a certain bitmap allows other nontrivial bitmaps satisfying condition (C1) is NP-hard, so it's impossible to create a fast solution that always finishes. In practice, however, the algorithm runs into contradictions surprisingly rarely.

## Algorithm
---
1. Read the input bitmap and count NxN patterns.
    i. (optional) Augment pattern data with rotations and reflections.
2. Create an array with the dimensions of the output (called "wave" in the source). Each element of this array represents a state of an NxN region in the output. A state of an NxN region is a superposition of NxN patterns of the input with boolean coefficients (so a state of a pixel in the output is a superposition of input colors with real coefficients). False coefficient means that the corresponding pattern is forbidden, true coefficient means that the corresponding pattern is not yet forbidden.
3. Initialize the wave in the completely unobserved state, i.e. with all the boolean coefficients being true.
4. Repeat the following steps:
    i. Observation:
        a. Find a wave element with the minimal nonzero entropy. If there is no such elements (if all elements have zero or undefined entropy) then break the cycle (4) and go to step (5).
        b. Collapse this element into a definite state according to its coefficients and the distribution of NxN patterns in the input.
    ii. Propagation: propagate information gained on the previous observation step.
5. By now all the wave elements are either in a completely observed state (all the coefficients except one being zero) or in the contradictory state (all the coefficients being zero). In the first case return the output. In the second case finish the work without returning anything.

## Tilemap Generation
---
The simplest nontrivial case of the algorithm is when NxN=1x2 (well, NxM). If we simplify it even further by storing not the probabilities of pairs of colors but the probabilities of colors themselves, we get what we call a "simple tiled model". The propagation phase in this model is just adjacency constraint propagation. It's convenient to initialize the simple tiled model with a list of tiles and their adjacency data (adjacency data can be viewed as a large set of very small samples) rather than a sample bitmap.

![[tile.gif]]

Lists of all the possible pairs of adjacent tiles in practical tilesets can be quite long, so I implemented a symmetry system for tiles to shorten the enumeration. In this system each tile should be assigned with its symmetry type.

![[symmetry-system.png]]

Note that the tiles have the same symmetry type as their assigned letters (or, in other words, actions of the dihedral group D4 are isomorphic for tiles and their corresponding letters). With this system it's enough to enumerate pairs of adjacent tiles only up to symmetry, which makes lists of adjacencies for tilesets with many symmetrical tiles (even the summer tileset, despite drawings not being symmetrical the system considers such tiles to be symmetrical) several times shorter.

## Higher Dimensions
---
WFC algorithm in higher dimensions works completely the same way as in dimension 2, though performance becomes an issue. These voxel models were generated with N=2 overlapping tiled model using 5x5x5 and 5x5x2 blocks and additional heuristics (height, density, curvature, …).

![[castles-3d.png]]

## Constrained Synthesis
---
WFC algorithm supports constraints. Therefore, it can be easily combined with other generative algorithms or with manual creation.

Here is WFC autocompleting a level started by a human:

![[constrained.gif]]

[[ConvChain]] algorithm satisfies the strong version of the condition (C2): the limit distribution of NxN patterns in the outputs it is producing is exactly the same as the distributions of patterns in the input. However, ConvChain doesn't satisfy (C1): it often produces noticeable defects. It makes sense to run ConvChain first to get a well-sampled configuration and then run WFC to correct local defects. This is similar to a common strategy in optimization: first run a [[Monte-Carlo method]] to find a point close to a global optimum and then run a gradient descent from that point for greater accuracy.