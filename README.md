The system gets as input data the video stream from a static camera and classifies road lanes as being
occupied or not given a video frame (Task 1) and tracka a specific vehicle in a given video (Task 2).

The videos are collected at different times during day/week so it is easy to observe that there are some changes in illumination in the scene specific to each video:

![alt text](https://github.com/StegarescuAnaMaria/Visual_Traffic_Monitoring_at_a_Road_Intersection_Computer_Vision/blob/main/images/1.png)

#Task 1:#
Road lanes are numbered from 1 to 3 for the upper left part of the intersection, 4 to 6 for the right part and 7 to 9 for the bottom right part of the intersection.
![alt text](https://github.com/StegarescuAnaMaria/Visual_Traffic_Monitoring_at_a_Road_Intersection_Computer_Vision/blob/main/images/2.png)

YOLOv8 (object detection model) was used for object (in this case vehicle) detection. Source: https://github.com/autogyro/yolo-V8/blob/main/examples/tutorial.ipynb 
YOLOv8 has a number of “classes” for object detection available, each class having an assigned number 
to it. 4 classes are taken into account (2, 3, 5, 7): {2: 'car', 3: 'motorcycle', 5: 'bus', 7: 'truck'}.
 
Firstly, I took the coordinates for each of the 9 lanes, by uploading some sample train images to 
https://nutbread.github.io/icm/ and selecting each point manually. The coordinates were hard-coded 
into a dictionary, the number of the lane being the key, and the array of “tuples” (x, y coord) being the 
value.

I used the “shapely” library, and created a polygon object as the lane, and a box object as the bounding box of the identified object, calculated the area of their intersection, divided it by the area of the bounding box, and set a threshold of 0.7 (70%): if the result of the division is bigger 
than 0.7, then (most) of the bounding box, if not all, is part of the polygon/lane. If true, the answer is 1, else 0, for the question “Is the x lane occupied by a vehicle for the y image?”.

#Task 2:#
