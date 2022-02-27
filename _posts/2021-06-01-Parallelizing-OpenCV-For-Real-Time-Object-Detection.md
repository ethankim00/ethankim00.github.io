---
title: "Parallelizing OpenCV For Real Time Object Detection"
last_modified_at: 2021-07-01T16:20:02-05:00
categories:
    - project
tags:
    - computer_vision
    - parallelization
---

## Introduction

Computer vision advances have enabled novel applications such as object detection and tracking.  Naive object tracking implementations operate at extremely slow speeds. My baseline implementation ran at .25 FPS on 4K images. Optimizing and speeding up an object detection algorithm to run in real time has applications realms such as sports analytics, medical scanning, surveillance and security, self-driving cars, and more. For this project we used the Histogram of Oriented Gradients for object detection and Kernelized Correlation Filter for object tracking. In addition, we experimented with Deep Learning Methods such as YOLOv3. 



## Methodology

Object tracking was implemented using the OpenCV package. Experiments were run on an AWS with 32 cores and 1 Tesla M60 GPU. For the tracking algorithms an initial set of pedestrians is first detected using the HOG object detector with an SVM trained to detect pedestrians. These objects are then tracked for the rest of the video using a Kernelized Correlation Filter or other object tracking algorithm. The key process of updating each object tracker with the new frame can be parallelized across threads using OpenMP. For example, when tracking 10 objects we could do the KCF update computations in 10 different threads thus vastly decreasing the runtime. In addition, we sped up the tracker by downsizing input video frames while taking advantage of faster speeds on the GPU. 

## Results

#### Comparison Of Object Tracking Algorithms

We compared the multithreaded implementations of the various image tracking algorithms in openCV. This verified the literature reported results that KCF tracking presented the best tradeoff between tracking quality and speed. We were not able to benchmark the GOTURN tracker available in openCV since this deep learning based algorithm required too much memory overhead to initialize multiple trackers. The table below shows benchmarks of the main tracking algorithms on a 4K video with multiple objects and 16 threads.

| Algorithm  | Multithreaded FPS |
| ---------- | ----------------- |
| KCF        | 2.5               |
| MOSSE      | 6.66              |
| BOOSTING   | 2.8               |
| MIL        | 1.5               |
| TLD        | 0.645             |
| MEDIANFLOW | 3.333             |
| CSRT       | 1.111             |

### OpenMP

Tracker updates were parallelized across different threads using OpenMP. The key code is shown below. 

```cpp
    #pragma omp parallel
    #pragma omp for
    for(unsigned i=0; i<bboxes.size(); i++){
        Rect2d temp = bboxes[i];
        found = trackers[i]->update(frame, temp);
        if (found){
            bboxes[i] = temp;
            rectangle(frame, bboxes[i], colors[i], 2, 1);
        }
    }
```

Varying numbers of threads from 0-16 were applied. The speedups achieved using different numbers of threads are shown below. Overall we were able to achieve a maximum 3.6x speedup using openMP.

![Results](/assets/images/opencv/openmptracking.png)

### Downsizing

To achieve the necessary speedup for realtime (30FPS) applications we downsized each image prior to running the object tracking algorithm. This image preprocessing step was sped up by over 2x by using the CUDA enabled GPU algorithm.


![Speedup](/assets/images/opencv/KCF%20Algorithm%20Speedup%20With%20Built-Up%20Features.png)

### YOLO Approach

An alternative to online object tracking algorithms is simply to treat each frame as independent and detect objects as they come in. This has the advantage of eliminating any dependencies on the previous video frame but does not track an individually identifiable object over time. We used the yolov3 pre-trained deep learning model with openCL support. The baseline implementation was able to run at approximately 5 fps with around 40% utilization of a single Tesla M60 GPU. Note the the openCL implementation was unable to take advantage of multiple GPUs. Unfortunately we did not get to try out the CUDA accelerated version since the cuDNN library would not work with the Tesla M60. The openCL version was actually slower than the CPU. However, we would expect much better performance using the GPU version with CUDA. Qualitatively, the yolo object detection gives different results than the tracking algorithms. Each frame is evaluated independently so there is less coherence in the location of detected objects across frames. We can also see how yolo is able to assign a class label to the objects it detects.

| Backend | FPS |
| ------- | --- |
| CPU     | 8   |
| openCL  | 5   |


## Demos

### Object Tracking

![Tracking](/assets/images/opencv/tracking.gif)

### Object Detection

![Detection](/assets/images/opencv/detection2.gif)

### Livestream

![LiveStream](/assets/images/opencv/livedemo.gif)