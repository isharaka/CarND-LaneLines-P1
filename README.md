#**Finding Lane Lines on the Road** 


Files
---

P1.ipnub - project code
writeup.md - project writeup
test_images_output/ - directory containing annotated test images
test_videos_output/ - directory containing annotated test videos
writeup_images/ - directory containing images at various stages of the pipeline


Instructions
---

The pipeline is implemented in the find_lanes function.


def find_lanes(image,debug=0):
    """
    `image` should be the input from the front camera
    `debug` non zero value of this will add extra annotations 
           (contours of lane marker segments and convex hull over them) on the output image
        
    Returns an image with lane markers drawn.
    """

Notes
---

The challenge video output is created with the debug option in find_images enabled.
