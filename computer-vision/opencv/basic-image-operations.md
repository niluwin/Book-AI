# Basic Image Operations

## Read image - `imread()`

[Link](https://learnopencv.com/read-display-and-write-an-image-using-opencv/)

`imread(filename, flags)`

It takes two arguments:

1. The first argument is the image name, which requires a fully qualified **pathname** to the file.
2. The second argument is an optional flag that lets you specify how the image should be represented. OpenCV offers several options for this flag, but those that are most common include:
   * `cv2.IMREAD_UNCHANGED`  or `-1`
   * `cv2.IMREAD_GRAYSCALE`  or `0`
   * `cv2.IMREAD_COLOR`  or `1`- _default_
   * To check out the different flag options, click [here](https://docs.opencv.org/master/d8/d6a/group\_\_imgcodecs\_\_flags.html#ga61d9b0126a3e57d9277ac48327799c80)

{% hint style="warning" %}
OpenCV reads color images in BGR _\*\*_format, whereas most other computer vision libraries use the RGB channel format order
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
# import the cv2 library
import cv2

# The function cv2.imread() is used to read an image.
img_grayscale = cv2.imread('dog.jpg',0)

#or
# Read an image
img_color = cv2.imread('test.jpg',cv2.IMREAD_COLOR)
img_grayscale = cv2.imread('test.jpg',cv2.IMREAD_GRAYSCALE)
img_unchanged = cv2.imread('test.jpg',cv2.IMREAD_UNCHANGED)

# The function cv2.imshow() is used to display an image in a window.
cv2.imshow('color image',img_color)  
cv2.imshow('grayscale image',img_grayscale)
cv2.imshow('unchanged image',img_unchanged)

# waitKey() waits for a key press to close the window and 0 specifies indefinite loop
cv2.waitKey(0)

# cv2.destroyAllWindows() simply destroys all the windows we created.
cv2.destroyAllWindows()
```
{% endtab %}

{% tab title="C++" %}
```cpp
//Include Libraries
#include<opencv2/opencv.hpp>
#include<iostream>

// Namespace nullifies the use of cv::function(); 
using namespace std;
using namespace cv;

// Read an image 
Mat img_grayscale = imread("test.jpg", 0);

//or
// Read an image 
Mat img_color = imread("test.jpg", IMREAD_COLOR);
Mat img_grayscale = imread("test.jpg", IMREAD_GRAYSCALE);
Mat img_unchanged = imread("test.jpg", IMREAD_UNCHANGED);

// Create a window.
namedWindow( "color image", WINDOW_AUTOSIZE );
namedWindow( "grayscale image", WINDOW_AUTOSIZE );
namedWindow( "unchanged image", WINDOW_AUTOSIZE );

// Show the image inside it.
imshow( "color image", img_color ); 
imshow( "grayscale image", img_grayscale );
imshow( "unchanged image", img_unchanged ); 

// Wait for a keystroke.   
waitKey(0);  

// Destroys all the windows created                         
destroyAllWindows();
```
{% endtab %}
{% endtabs %}

## Display image - `imshow()`

`imshow(window_name, image)`

1. The first argument is the window name that will be displayed on the window.
2. The second argument is the image that you want to display.&#x20;

To display multiple images at once, specify a new window name for every image you want to display.

The `imshow()` function is designed to be used along with the `waitKey()` and `destroyAllWindows()` / `destroyWindow()` functions.

The `waitKey()` function is a keyboard-binding function.

* It takes a single argument, which is the **time (in milliseconds)**, for which the window will be displayed.
* If the user presses any key within this time period, the program continues.
* If **0** is passed, the program waits indefinitely for a keystroke.
* You can also set the function to detect specific keystrokes like the **Q** key or the **ESC** key on the keyboard, thereby telling more explicitly which key shall trigger which behavior.&#x20;

The function `destroyAllWindows()` destroys all the windows we created. If a specific window needs to be destroyed, give that exact window name as the argument. Using `destroyAllWindows()` also clears the window or image from the main memory of the system.

## Write image - `imwrite()`

`imwrite(filename, image)`

1. The first argument is the filename, which must include the filename extension (for example .png, .jpg etc). OpenCV uses this filename extension to specify the format of the file.&#x20;
2. The second argument is the image you want to save. The function returns `True` if the image is saved successfully.

{% tabs %}
{% tab title="Python" %}
```python
cv2.imwrite('grayscale.jpg',img_grayscale)
```
{% endtab %}

{% tab title="C++" %}
```cpp
imwrite("grayscale.jpg", img_grayscale);
```
{% endtab %}
{% endtabs %}

## Resize image - `resize()`

[Link](https://learnopencv.com/image-resizing-with-opencv/)

* It is important to keep in mind the original aspect ratio of the image (i.e. width by height), if you want to maintain the same in the resized image too.
* Reducing the size of an image will require resampling of the pixels.&#x20;
* Increasing the size of an image requires reconstruction of the image. This means you need to interpolate new pixels.

### `using height and width`

{% tabs %}
{% tab title="Python" %}
```python
# let's start with the Imports 
import cv2
import numpy as np

# Read the image using imread function
image = cv2.imread('image.jpg')
cv2.imshow('Original Image', image)

# let's downscale the image using new  width and height
down_width = 300
down_height = 200
down_points = (down_width, down_height)
resized_down = cv2.resize(image, down_points, interpolation= cv2.INTER_LINEAR)

# let's upscale the image using new  width and height
up_width = 600
up_height = 400
up_points = (up_width, up_height)
resized_up = cv2.resize(image, up_points, interpolation= cv2.INTER_LINEAR)

# Display images
cv2.imshow('Resized Down by defining height and width', resized_down)
cv2.waitKey()
cv2.imshow('Resized Up image by defining height and width', resized_up)
cv2.waitKey()

#press any key to close the windows
cv2.destroyAllWindows()
```
{% endtab %}

{% tab title="C++" %}
```cpp
// let's start with including libraries 
#include<opencv2/opencv.hpp>
#include<iostream>

// Namespace to nullify use of cv::function(); syntax
using namespace std;
using namespace cv;

int main()
{
    // Read the image using imread function
    Mat image = imread("image.jpg");
    imshow("Original Image", image);


    // let's downscale the image using new  width and height
    int down_width = 300;
    int down_height = 200;
    Mat resized_down;
    //resize down
    resize(image, resized_down, Size(down_width, down_height), INTER_LINEAR);
    // let's upscale the image using new  width and height
    int up_width = 600;
    int up_height = 400;
    Mat resized_up;
    //resize up
    resize(image, resized_up, Size(up_width, up_height), INTER_LINEAR);
    // Display Images and press any key to continue
    imshow("Resized Down by defining height and width", resized_down);
    waitKey();
    imshow("Resized Up image by defining height and width", resized_up);
    waitKey();


    destroyAllWindows();
    return 0;
}
```
{% endtab %}
{% endtabs %}

To obtain the size of an image:

* use the **shape** method in **Python**
  * `image.shape` in  returns three values: **(H, W, C)**
* **rows** and **cols** in **C++**&#x20;
  * `image.rows` gives you the height
  * `image.columns` gives you the width of the image&#x20;
  * using the `size()` function
    * `image.size().width` returns the width
    * `image.size().height` returns the height

{% tabs %}
{% tab title="Python" %}
```python
# Get original height and width
h,w,c = image.shape
print("Original Height and Width:", h,"x", w)
```
{% endtab %}

{% tab title="C++" %}
```cpp
// Get height and width
cout << "Original Height and Width :" << image.rows << "x" << image.cols << endl;
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
OpenCV outputs the shape of an image in ![height \* width \* channels ](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-29d033d57c7330f89d8885fbc83a9834\_l3.png) format, whereas some other image-processing libraries give in the form of _**width, height, channels.**_ There’s a logical take to this.
{% endhint %}

> When images are read using OpenCV, they are represented as NumPy arrays. And in general, you always refer to the shape of an array, in terms of ![rows \* columns](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-9e8744fadcaf9eeeb4be4a5ac2edf8c3\_l3.png) (rows representing its height and the columns its width). So, even when reading images with OpenCV to get their shape, the same NumPy array rule comes into play. And you get the shape in the form of ![height \* width \* channels](https://learnopencv.com/wp-content/ql-cache/quicklatex.com-34e0ab84b7d34817c20bb347dfbf59e7\_l3.png).

`resize(src, dsize[, dst[, fx[, fy[, interpolation]]]])`

* **`src`**: It is the required input image, it could be a string with the path of the input image (eg: ‘test\_image.png’).
* **`dsize`**: It is the desired size of the output image, it can be a new height and width.
* **`fx`**: Scale factor along the horizontal axis.
* **`fy`**: Scale factor along the vertical axis.
* **`interpolation`**: It gives us the option of different methods of resizing the image.

### Using scaling factor

{% tabs %}
{% tab title="Python" %}
```python
# Scaling Down the image 1.2 times by specifying both scaling factors
scale_up_x = 1.2
scale_up_y = 1.2
# Scaling Down the image 0.6 times specifying a single scale factor.
scale_down = 0.6

scaled_f_down = cv2.resize(image, None, fx= scale_down, fy= scale_down, interpolation= cv2.INTER_LINEAR)
scaled_f_up = cv2.resize(image, None, fx= scale_up_x, fy= scale_up_y, interpolation= cv2.INTER_LINEAR)
```
{% endtab %}

{% tab title="C++" %}
```cpp
// Scaling Down the image 1.2 times by specifying both scaling factors
double scale_up_x = 1.2;
double scale_up_y = 1.2;
// Scaling Down the image 0.6 times specifying a single scale factor.
double scale_down = 0.6;
Mat scaled_f_up, scaled_f_down;
//resize 
resize(image,scaled_f_down, Size(), scale_down, scale_down, INTER_LINEAR);
resize(image, scaled_f_up, Size(), scale_up_x, scale_up_y, INTER_LINEAR);
```
{% endtab %}
{% endtabs %}

In the above **Python** snippet:

* Define new scaling factors along the **horizontal** and **vertical** axis.&#x20;
* Defining the scaling factors removes the need to have new points for width and height. Hence, we keep _**`dsize`**_ as **`None`.**&#x20;

In the above **C++** snippet:

* Define the new scaling factors as well as the matrices for the new images.
* As we do not need new points for width and height, we keep **`Size()`** empty and use the **`resize()`** function.&#x20;

### Interpolation Methods

Different interpolation methods are used for different resizing purposes.

* **`INTER_AREA`:** `INTER_AREA` uses pixel area relation for resampling. This is best suited for reducing the size of an image (**shrinking**). When used for zooming into the image, it uses the `INTER_NEAREST` method.
* **`INTER_CUBIC`:** This uses bicubic interpolation for resizing the image. While resizing and interpolating new pixels, this method acts on the 4×4 neighboring pixels of the image. It then takes the weights average of the 16 pixels to create the new interpolated pixel.
* **`INTER_LINEAR`**: This method is somewhat similar to the `INTER_CUBIC` interpolation. But unlike `INTER_CUBIC`, this uses 2×2 neighboring pixels to get the weighted average for the interpolated pixel.
* **`INTER_NEAREST`**: The `INTER_NEAREST` method uses the nearest neighbor concept for interpolation. This is one of the simplest methods, using only one neighboring pixel from the image for interpolation.

## Concatenation of images

{% tabs %}
{% tab title="Python" %}
```python
# Concatenate images in horizontal axis for comparison
vertical= np.concatenate((res_inter_nearest, res_inter_linear, res_inter_area), axis = 0)
# Display the image Press any key to continue
cv2.imshow('Inter Nearest :: Inter Linear :: Inter Area', vertical)
```
{% endtab %}

{% tab title="C++" %}
```cpp
Mat a,b,c;
vconcat(res_inter_linear, res_inter_nearest, a);
vconcat(res_inter_area, res_inter_area, b);
vconcat(a, b, c);
// Display the image Press any key to continue
imshow("Inter Linear :: Inter Nearest :: Inter Area :: Inter Area", c);
```
{% endtab %}
{% endtabs %}

Vertical concatenation - `axis = 0`

Horizontal concatenation - `axis = 1`

## Crop image

[Link](https://learnopencv.com/cropping-an-image-using-opencv/)

{% tabs %}
{% tab title="Python" %}
```python
# Import packages
import cv2
import numpy as np

img = cv2.imread('test.jpg')
print(img.shape) # Print image shape
cv2.imshow("original", img)

# Cropping an image
cropped_image = img[80:280, 150:330]

# Display cropped image
cv2.imshow("cropped", cropped_image)

# Save the cropped image
cv2.imwrite("Cropped Image.jpg", cropped_image)

cv2.waitKey(0)
cv2.destroyAllWindows()
```
{% endtab %}

{% tab title="C++" %}
```cpp
// Include Libraries
#include<opencv2/opencv.hpp>
#include<iostream>

// Namespace nullifies the use of cv::function();
using namespace std;
using namespace cv;

int main()
{
    // Read image
    Mat img = imread("test.jpg");
    cout << "Width : " << img.size().width << endl;
    cout << "Height: " << img.size().height << endl;
    cout<<"Channels: :"<< img.channels() << endl;
    // Crop image
    Mat cropped_image = img(Range(80,280), Range(150,330));

    //display image
    imshow(" Original Image", img);
    imshow("Cropped Image", cropped_image);

    //Save the cropped Image
    imwrite("Cropped Image.jpg", cropped_image);

    // 0 means loop infinitely
    waitKey(0);
    destroyAllWindows();
    return 0;
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
In OpenCV, you crop the image using the same method as **NumPy** array slicing.
{% endhint %}

* The **first** dimension is always the number of **rows** or the **height** of the image.
* The **second** dimension is the number of **columns** or the **width** of the image.&#x20;

### **Dividing an Image Into Small Patches Using Cropping**

{% tabs %}
{% tab title="Python" %}
```python
img =  cv2.imread("test_cropped.jpg")
image_copy = img.copy() 
imgheight=img.shape[0]
imgwidth=img.shape[1]

M = 76
N = 104
x1 = 0
y1 = 0

for y in range(0, imgheight, M):
    for x in range(0, imgwidth, N):
        if (imgheight - y) < M or (imgwidth - x) < N:
            break

        y1 = y + M
        x1 = x + N

        # check whether the patch width or height exceeds the image width or height
        if x1 >= imgwidth and y1 >= imgheight:
            x1 = imgwidth - 1
            y1 = imgheight - 1
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)
        elif y1 >= imgheight: # when patch height exceeds the image height
            y1 = imgheight - 1
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)
        elif x1 >= imgwidth: # when patch width exceeds the image width
            x1 = imgwidth - 1
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)
        else:
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)

#Save full image into file directory
cv2.imshow("Patched Image",img)
cv2.imwrite("patched.jpg",img)

cv2.waitKey()
cv2.destroyAllWindows()
```
{% endtab %}

{% tab title="C++" %}
```cpp
Mat img = imread("test_cropped.jpg");
Mat image_copy = img.clone();
int imgheight = img.rows;
int imgwidth = img.cols;

int M = 76;
int N = 104;

int x1 = 0;
int y1 = 0;
for (int y = 0; y<imgheight; y=y+M)
{
    for (int x = 0; x<imgwidth; x=x+N)
    {
        if ((imgheight - y) < M || (imgwidth - x) < N)
        {
            break;
        }
        y1 = y + M;
        x1 = x + N;
        string a = to_string(x);
        string b = to_string(y);

        if (x1 >= imgwidth && y1 >= imgheight)
        {
            x = imgwidth - 1;
            y = imgheight - 1;
            x1 = imgwidth - 1;
            y1 = imgheight - 1;

            // crop the patches of size MxN
            Mat tiles = image_copy(Range(y, imgheight), Range(x, imgwidth));
            //save each patches into file directory
            imwrite("saved_patches/tile" + a + '_' + b + ".jpg", tiles);  
            rectangle(img, Point(x,y), Point(x1,y1), Scalar(0,255,0), 1);    
        }
        else if (y1 >= imgheight)
        {
            y = imgheight - 1;
            y1 = imgheight - 1;

            // crop the patches of size MxN
            Mat tiles = image_copy(Range(y, imgheight), Range(x, x+N));
            //save each patches into file directory
            imwrite("saved_patches/tile" + a + '_' + b + ".jpg", tiles);  
            rectangle(img, Point(x,y), Point(x1,y1), Scalar(0,255,0), 1);    
        }
        else if (x1 >= imgwidth)
        {
            x = imgwidth - 1;   
            x1 = imgwidth - 1;

            // crop the patches of size MxN
            Mat tiles = image_copy(Range(y, y+M), Range(x, imgwidth));
            //save each patches into file directory
            imwrite("saved_patches/tile" + a + '_' + b + ".jpg", tiles);  
            rectangle(img, Point(x,y), Point(x1,y1), Scalar(0,255,0), 1);    
        }
        else
        {
            // crop the patches of size MxN
            Mat tiles = image_copy(Range(y, y+M), Range(x, x+N));
            //save each patches into file directory
            imwrite("saved_patches/tile" + a + '_' + b + ".jpg", tiles);  
            rectangle(img, Point(x,y), Point(x1,y1), Scalar(0,255,0), 1);    
        }
    }
}

imshow("Patched Image", img);
imwrite("patched.jpg",img);
waitKey();
destroyAllWindows();
```
{% endtab %}
{% endtabs %}
