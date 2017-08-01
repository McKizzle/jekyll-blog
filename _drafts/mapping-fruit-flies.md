

Question? Extracting meaning from height dimensional data. 

Goal: Using high res image data of fly brains map neuron activity. 

- opto/thermogenetics
-  functional imaging & electrophysiology

# Neural Activation Screen
 - Janelia GAL4 collection of transgenic lines of flies. 

 - What behaviors do flies do?
  - behavior classification.
 - how should we represent their behavior?
 -- Data mining
 - how do we extract this representation from video?
 -- Data mining

 # training
  - Provide a small set of frames for each type of behavior. 
  - Apply the classifier to the video. 
  - JAABA interactive framework
  - JAABA cannot track animals. Tracking data is required. 
  - CTRACK?

# Features
  - computer 'per-frame' time series features.
  - Features
    - 

# TRaining
  - each labeled frame is treated as an independent training example
  - learn a boosted decision stumps-based classifier.

# Classifier Accuracy

# Fly bowl neural activation screen
  - Tracked all of the flies 
    - Bodies
    - Windws
  - attempted copuation
  - wing extension
  - chase
  - touch
  - wing grooming 
  - wing  flick
  - righting
  - jump
  - Change in orientation
  - speed
  - walk
  - stop
  - crab walk
  - pivot-tail
  - back-up
  - distance to wall
  - pivot-center

# Generalization
  - Difficulties
    - Behavior of different animals differs greatly. Cannot use single classifier. 
  - Train an initial classifier and some known behavioral outliers. 
  - Run the classifier on the entire collection. 

# Projections
  - Tee Snee Projection
    - constraints?

#  Average expression per cluster
  - 30 million voxels = 30 million hypothesis tests > p-value < 10^-5
  - Difficult if
    - registration, image processing is inexact
    - biology is messy - stochastic expression patterns

# 
