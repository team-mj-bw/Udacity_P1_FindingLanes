# **Finding Lane Lines on the Road - Reflection on Project**

[Gray_with_Gauss]: ./test_videos_output/gray_with_gauss.png
[canny]: ./test_videos_output/canny.png
[area_of_interest]: ./test_videos_output/area_of_interest.png
[final]: ./test_videos_output/final.png
[lane_markers]: ./test_videos_output/line_markings.jpg


## 1. Description of pipeline

My pipeline consisted of 5 steps:

1. Convert image (copy of image) to black and white and apply gaussian blur to reduce noise in the picture

![Gray with gauss][Gray_with_Gauss]

2. Apply canny function on the output of gaussian blur

![canny][canny]

3. "Cut" out area of interest

![area of interest][area_of_interest]

4. Identify lane lines using hough line transformation and map them on picture

![lane markers][lane_markers]

This worked fine on the example pictures, but when I tried the videos the middle lanes were not well maped. Therefore after a while of playing around on the video I extracted frame by frame from the "yellow" video to work on the problematic frames. I noticed that the problem was only partially the configuration of the hough transformation but my area of interest was to narrow. It took me a while to get it to an acceptable point.

5. Extrapolate line to have one full line and draw them

My approach was to first separate left and right lane pieces. Then I would take the median of the line parameters (m,b) to fit the line. Lastly I extrapolate the "fitted" line from the bottom to the end of the area of interest. This approach worked quite well, but the line was a bit jumpy from the left side of the lane to the right. Therefore I tried a different approach by using the polyfit. Here I had problems with outliers in the identified lane pieces. I tried to exclude these outliers, but after playing around quite a while, I decided to go back to my first approach. I smoothened the lines a bit by not only considering frame by frame, but taking the last three frames. In reality this would make the car react more slowly to changes, but it would be more robust.

At the end the lines are still a bit jumpy, but for now I find the result sufficient.

![final_picture][final]

---
### 2. Shortcomings of my pipeline

Like mentioned before the lanes are still a bit jumpy and also the identification of lane segments could be further enhanced by playing around even more with the canny and hough parameters. But those are minor problems. The larger issues with my pipeline become apparent when trying the challenge video. Because this includes a curve my pipeline in current configuration is basically useless. The problem already starts with my current polygon to cut out the area of interest. When facing a more curved lane line in front of us, the region by definition cuts out large parts of one side of the lane. Also I am also not sure if the hough line transformation should be used for a more round lane shape. Lastly to get one line I am using straight line equations which also might be wrong.

---
### 3. Suggest possible improvements to your pipeline

I am sure my current pipeline and its input factors could be optimized further. Maybe you could implement an algorithm that automatically tries several configuration. The problem here is to automize the evaluation of the results. Maybe by analyzing the extracted hough lines in terms of total lenght of the lines and how much the gradient differs (measure of relevant lane lines) we could implement something helpful.

To make the pipeline more robust and useful in curves I feel like there is a lot to be changed. Starting by the way I identified the region of interest and also the hough identification of lines. I found that hough can also detect circles and the function polyfit can also fit other functions. Maybe those "tools" could help. It is also possible that I am thinking too  complex and it is more about tweaking the pipeline. I might play around a bit more to see if where the real shortcomings of my pipeline for the challenge videos are. I would go back and use single frames to test each part of my pipeline.
