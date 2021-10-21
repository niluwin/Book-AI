# Basic Video Operations

[Link](https://learnopencv.com/reading-and-writing-videos-using-opencv/)

Reading and writing videos in OpenCV is very similar to reading and writing images. A video is nothing but a series of images that are often referred to as _frames_. So, all you need to do is loop over all the frames in a video sequence, and then process one frame at a time.

{% tabs %}
{% tab title="Python" %}
```python
import cv2 

# Create a video capture object, in this case we are reading the video from a file
vid_capture = cv2.VideoCapture('Resources/Cars.mp4')

if (vid_capture.isOpened() == False):
    print("Error opening the video file")
# Read fps and frame count
else:
    # Get frame rate information
    # You can replace 5 with CAP_PROP_FPS as well, they are enumerations
    fps = vid_capture.get(5) # CAP_PROP_FPS
    print('Frames per second : ', fps,'FPS')

    # Get frame count
    # You can replace 7 with CAP_PROP_FRAME_COUNT as well, they are enumerations
    frame_count = vid_capture.get(7) # CAP_PROP_FRAME_COUNT
    print('Frame count : ', frame_count)

while(vid_capture.isOpened()):
    # vid_capture.read() methods returns a tuple, first element is a bool 
    # and the second is frame
    ret, frame = vid_capture.read()
    if ret == True:
        cv2.imshow('Frame',frame)
        # 20 is in milliseconds, try to increase the value, say 50 and observe
        key = cv2.waitKey(20)

        if key == ord('q'):
            break
    else:
        break

# Release the video capture object
vid_capture.release()
cv2.destroyAllWindows()
```
{% endtab %}

{% tab title="C++" %}
```cpp
// Include Libraries
#include<opencv2/opencv.hpp>
#include<iostream>

// Namespace to nullify use of cv::function(); syntax
using namespace std;
using namespace cv;

int main()
{
    // initialize a video capture object
    VideoCapture vid_capture("Resources/Cars.mp4");

    // Print error message if the stream is invalid
    if (!vid_capture.isOpened())
    {
        cout << "Error opening video stream or file" << endl;
    }

    else
    {
        // Obtain fps and frame count by get() method and print
        // You can replace 5 with CAP_PROP_FPS as well, they are enumerations
        int fps = vid_capture.get(5);
        cout << "Frames per second :" << fps;

        // Obtain frame_count using opencv built in frame count reading method
        // You can replace 7 with CAP_PROP_FRAME_COUNT as well, they are enumerations
        int frame_count = vid_capture.get(7);
        cout << "  Frame count :" << frame_count;
    }


    // Read the frames to the last frame
    while (vid_capture.isOpened())
    {
        // Initialise frame matrix
        Mat frame;

        // Initialize a boolean to check if frames are there or not
        bool isSuccess = vid_capture.read(frame);

        // If frames are present, show it
        if(isSuccess == true)
        {
            //display frames
            imshow("Frame", frame);
        }

        // If frames are not there, close it
        if (isSuccess == false)
        {
            cout << "Video camera is disconnected" << endl;
            break;
        }

        //wait 20 ms between successive frames and break the loop if key q is pressed
        int key = waitKey(20);
        if (key == 'q')
        {
            cout << "q key is pressed by the user. Stopping the video" << endl;
            break;
        }


    }
    // Release the video capture object
    vid_capture.release();
    destroyAllWindows();
    return 0;
}
```
{% endtab %}
{% endtabs %}

## Read Video

1. `cv2.VideoCapture` – Creates a video capture object, which would help stream or display the video.
2. `cv2.VideoWriter` – Saves the output video to a directory.
3. `get()`is used to read the video metadata such as frame height, width, fps etc. [List](https://docs.opencv.org/master/d4/d15/group\_\_videoio\_\_flags\_\_base.html#gaeb8dd9c89c10a5c63c139bf7c4f5704d)

{% hint style="warning" %}
`get()` method does not apply to web cameras.
{% endhint %}

### From a file

`VideoCapture(path, apiPreference)`

`VideoCapture()` class to create a [VideoCapture](https://docs.opencv.org/4.5.2/d8/dfe/classcv\_1\_1VideoCapture.html#ac4107fb146a762454a8a87715d9b7c96) object, which we will then use to read the video file.

1. The first argument is the filename/path to the video file.&#x20;
2. The second is an optional argument, indicating an `API` [preference.](https://docs.opencv.org/master/d4/d15/group\_\_videoio\_\_flags\_\_base.html)

The `isOpened()` method returns a **boolean** that indicates whether or not the video stream is valid. Otherwise you will get an **error** message. The error message can imply many things. One of them is that the entire video is corrupted, or some frames are corrupted.

Once the video stream is fully processed or the user prematurely exits the loop, you release the video-capture object (`vid_capture`) and close the window

### From Image-Sequence

* Using the notation shown below (Cars%04d.jpg), where %04d indicates a four-digit sequence-naming convention (e.g. Cars0001.jpg, Cars0002.jpg, Cars0003.jpg, etc). &#x20;
* If you had specified “Race\_Cars\_%02d.jpg” then you would be looking for files of the form: (Race\_Cars\_01.jpg, Race\_Cars\_02.jpg, Race\_Cars\_03.jpg, etc…).

{% tabs %}
{% tab title="Python" %}
```python
vid_capture = cv2.VideoCapture('Resources/Image_sequence/Cars%04d.jpg')
```
{% endtab %}

{% tab title="C++" %}
```cpp
VideoCapture vid_capture("Resources/Image_sequence/Cars%04d.jpg");
```
{% endtab %}
{% endtabs %}

### From Webcam

Video capture class in OpenCV, has several overloaded functions for convenience that accept different input arguments

{% tabs %}
{% tab title="Python" %}
```python
vid_capture = cv2.VideoCapture(0, cv2.CAP_DSHOW)
```
{% endtab %}

{% tab title="C++" %}
```cpp
VideoCapture vid_capture(0);
```
{% endtab %}
{% endtabs %}

Flag **`CAP_DSHOW`** is an optional argument, short for directshow via video input

## Display Video

## Write Video

* Retrieve the image frame height and width, using the `get()` method.
* Initialize a video capture object (as discussed in the previous sections), to read the video stream into memory, using any of the sources previously described.
* Create a video writer object.
* Use the video writer object to save the video stream to disk.&#x20;

{% tabs %}
{% tab title="Python" %}
```python
# Obtain frame size information using get() method
frame_width = int(vid_capture.get(3))
frame_height = int(vid_capture.get(4))
frame_size = (frame_width,frame_height)
fps = 20

# Initialize video writer object
output = cv2.VideoWriter('Resources/output_video_from_file.avi', cv2.VideoWriter_fourcc('M','J','P','G'), 20, frame_size)

#Writing to file
while(vid_capture.isOpened()):
    # vid_capture.read() methods returns a tuple, first element is a bool 
    # and the second is frame

    ret, frame = vid_capture.read()
    if ret == True:
           # Write the frame to the output files
           output.write(frame)
    else:
         print(‘Stream disconnected’)
           break
```
{% endtab %}

{% tab title="C++" %}
```cpp
// Obtain frame size information using get() method
Int frame_width = static_cast<int>(vid_capture.get(3));
int frame_height = static_cast<int>(vid_capture.get(4));
Size frame_size(frame_width, frame_height);
int fps = 20;
//Initialize video writer object
VideoWriter output("Resources/output.avi", VideoWriter::fourcc('M', 'J', 'P', 'G'),frames_per_second, frame_size);

#Writing to file
while (vid_capture.isOpened())
{
        // Initialize frame matrix
        Mat frame;

          // Initialize a boolean to check if frames are there or not
        bool isSuccess = vid_capture.read(frame);

        // If frames are not there, close it
        if (isSuccess == false)
        {
            cout << "Stream disconnected" << endl;
            break;
        }


            // If frames are present
        if(isSuccess == true)
        {
            //display frames
            output.write(frame);
                  // display frames
                  imshow("Frame", frame);

                  // wait for 20 ms between successive frames and break        
                  // the loop if key q is pressed
                  int key = waitKey(20);
                  if (key == ‘q’)
                  {
                      cout << "Key q key is pressed by the user. 
                      Stopping the video" << endl;
                      break;
                  }
        }
 }
```
{% endtab %}
{% endtabs %}

Syntax of VideoWriter():

`VideoWriter(filename, apiPreference, fourcc, fps, frameSize[, isColor])`

* `filename`: pathname for the output video file
* `apiPreference`:  API backends identifier
* `fourcc`: 4-character code of codec, used to compress the frames ([fourcc](https://docs.opencv.org/4.5.2/dd/d9e/classcv\_1\_1VideoWriter.html#ad59c61d8881ba2b2da22cff5487465b5))
  * AVI: `cv2.VideoWriter_fourcc('M','J','P','G')`
  * MP4: `cv2.VideoWriter_fourcc(*'XVID')`
* `fps`: Frame rate of the created video stream
* `frame_size`: Size of the video frames
* `isColor`: If not zero, the encoder will expect and encode color frames. Else it will work with grayscale frames (the flag is currently supported on Windows only).
