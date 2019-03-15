**For accessing the data set please refere to [this](https://web.gin.g-node.org/ioannis.agtzidis/360_em_dataset) directory.**

Here we provide 15 360-degree equirectangular videos, togeher with eye tracking
recordings of 13 subjects and a manually labelled ground-truth subset of all
gaze recordings. Finally we also provide a algorithmic implemetntation of the
the process that was followed during manual labelling.

## 1. CONTENT

Before starting using the provided algorithms of this repository you should
first clone (or download) the *matlab_utils* repository that offers utilities
for mainly handling ARFF files from
[here](https://web.gin.g-node.org/ioannis.agtzidis/matlab_utils). We also
need to clone the *matlab_360_utils* repository that offers utilities for handling 360-degree
data from [here](https://web.gin.g-node.org/ioannis.agtzidis/matlab_360_utils). Then add the 
previous folders to the search path of Matlab with the *pathtool* or *addpath*
commands.

### 1.1 Video Stimuli

We used as an approximation to naturalistic stimuli 14 Youtube
videos from diverse contexts. The videos were published under the Creative
Commons license and we give attribution to the original creators by attaching
the Youtude IDS at the end of each video clip. 

A single syntetic stimulus was created by the authors and comprises of a moving
target, which tries to elicit many different kinds of eye motion (i.e.
fixations, saccade, SP, head pursuit, VOR, OKN). 

Files found in `videos`.

### 1.2 Gaze Recordings

In total 13 subjects participated in our study, which amounts to ca. 3.5 hours
of eye tracking data. Information about the participants can be found
[here](https://web.gin.g-node.org/ioannis.agtzidis/360_em_dataset/src/master/participant_info.csv).

Gaze files are found in `gaze` folder.

### 1.3 Ground Truth

we manually labelled part of the full data set accroding to the rules presented
in our paper. The labelled gaze recordings were split in two non overlapping
(subject wise) subsets, where one can be use as training and the other as
testing. In total the hand-lablled portion comprise of 2 labelled gaze files per
video stimulus and about 16 % of the data.

Manually annotated ground-truthf files are found in `ground_truth` folder.

### 1.4 Manual Labelling Interface

The manual labelling interface can be found
in the [GTA-VI](https://web.gin.g-node.org/ioannis.agtzidis/gta-vi) repository.
The GTA-VI repository contains the extension that was developed for labelling
this data set and enables labelling of 360-degree equirectangular recordings with our
two tier method.

### 1.5 Algorithmic Implemetation

We provide an algorithmic implementation for eye movement classification based
on the definitions that we provied in our paper. The resulting eye movements
adter applying our algorithms to ground truth files can be foundin the relevant
files.

Algorithms and their output are found in `em_algorithms`, `output_I-S5T_combined`, `output_I-S5T_FOV`, `output_I-S5T_E+H` folders.

## 2. DATA FORMAT

All the function use the ARFF data format for input and output to the disk. The initial
ARFF format was extended as described in Agtzidis et al. (2016) and was further expanded 
for 360-degree gaze data.

Here the "@RELATION" is set to gaze_360 to distinguish the recordings from
plain gaze recordings. We also make use of the "%@METADATA" special comments
which describe the field of view of the used headset. Apart from the default
metadata *width_px, height_px, distance_mm, width_mm, height_mm* we also use 
the extra metadata *fov_width_px, fov_width_deg, fov_height_px, fov_height_deg* 
that describe the headset properties.

The "@ATTRIBUTE" *x* and *y* represent the gaze coordinates in the
equirectangular space (i.e. the head and eye position together). The
*x_head* and *y_head* attributes represent head coordinates in the
equirectangular space. The *angle_deg_head* attribute is equivalent to the roll
principal axis.

### 2.1 ARFF Example

```
@RELATION gaze_360

%@METADATA distance_mm 0.00
%@METADATA height_mm 0.00
%@METADATA height_px 1080
%@METADATA width_mm 0.00
%@METADATA width_px 1920

%@METADATA fov_height_deg 100.00
%@METADATA fov_height_px 1440
%@METADATA fov_width_deg 100.00
%@METADATA fov_width_px 1280

@ATTRIBUTE time INTEGER
@ATTRIBUTE x NUMERIC
@ATTRIBUTE y NUMERIC
@ATTRIBUTE confidence NUMERIC
@ATTRIBUTE x_head NUMERIC
@ATTRIBUTE y_head NUMERIC
@ATTRIBUTE angle_deg_head NUMERIC
@ATTRIBUTE labeller_1 {unassigned,fixation,saccade,SP,noise,VOR,OKN}


@DATA
0,960.00,540.00,1.00,960.00,540.00,1.22,fixation
5000,959.00,539.00,1.00,959.00,539.00,1.23,fixation
13000,959.00,539.00,1.00,959.00,539.00,1.23,fixation
18000,959.00,539.00,1.00,959.00,539.00,1.23,fixation
29000,959.00,539.00,1.00,959.00,539.00,1.24,fixation
34000,959.00,539.00,1.00,959.00,539.00,1.24,fixation
45000,959.00,539.00,1.00,959.00,539.00,1.24,fixation
49000,959.00,539.00,1.00,959.00,539.00,1.24,fixation
61000,959.00,539.00,1.00,959.00,539.00,1.24,fixation
66000,959.00,539.00,1.00,959.00,539.00,1.24,fixation
77000,959.00,539.00,1.00,959.00,540.00,1.24,fixation
82000,959.00,539.00,1.00,959.00,540.00,1.24,fixation
94000,959.00,539.00,1.00,960.00,540.00,1.24,fixation
99000,959.00,539.00,1.00,960.00,540.00,1.24,fixation
110000,959.00,539.00,1.00,960.00,540.00,1.25,fixation
114000,959.00,539.00,1.00,960.00,540.00,1.25,fixation
125000,958.00,538.00,1.00,960.00,540.00,1.26,saccade
129000,956.00,537.00,1.00,960.00,540.00,1.27,saccade
141000,948.00,530.00,1.00,960.00,540.00,1.28,saccade
```

### 2.2 Recovery of HMD Pose

The *x_head, y_head, angle_deg_head* allow us to recover the headset pose
because we use 360-degree equirectangular videos and therefore exist no
translations during video presentation. For
understanding the process take a look at functions
[HeadToVideoRot.m](https://web.gin.g-node.org/ioannis.agtzidis/matlab_360_utils/src/master/HeadToVideoRot.m) and
[YZXrotation.m](https://web.gin.g-node.org/ioannis.agtzidis/matlab_360_utils/src/master/YZXrotation.m)
in the
[matlab_360_utils](https://web.gin.g-node.org/ioannis.agtzidis/matlab_360_utils) repository.

## 3. General Information

Author: Ioannis Agtzidis

Contact: ioannis.agtzidis@tum.de
