# Lane Line Detection in Videos

This project implements a computer vision pipeline to detect lane lines in video streams using Python, OpenCV, and MoviePy. It processes video frames, identifies lane boundaries, and overlays them onto the original video, creating an output video with detected lanes.

## Features

*   **Grayscale Conversion:** Converts RGB frames to grayscale for simpler processing.
*   **Gaussian Blur:** Reduces noise in images to improve edge detection accuracy.
*   **Canny Edge Detection:** Identifies strong edges in the processed frames.
*   **Region of Interest (ROI) Selection:** Focuses the analysis on a specific polygonal area of the road, ignoring irrelevant parts of the image.
*   **Hough Transform:** Detects straight lines within the defined ROI, which represent potential lane lines.
*   **Lane Line Averaging:** Calculates the average slope and intercept for left and right lane lines to create smooth and continuous lane markings.
*   **Video Processing:** Integrates all steps to process an entire video file and save the output.

## Installation

To run this project, you need to install the following Python libraries:

```bash
pip install opencv-python moviepy numpy pandas
```

## Usage

1.  **Prepare your input video:** Place your input video file (e.g., `input.mp4`) in the same directory as your script or provide its full path.
2.  **Run the script:** The `process_video` function is the main entry point.

    ```python
    # Example of how to call the main function
    process_video('input.mp4', 'output.mp4')
    ```

    *   `'input.mp4'`: Path to your source video file.
    *   `'output.mp4'`: Desired path for the output video with detected lane lines.

The script will generate an `output.mp4` file in the specified location, showing the original video with red lane lines overlaid.

## Code Structure

*   `frame_processor(image)`: The core function that takes a single image frame, applies the entire lane detection pipeline (grayscale, blur, Canny, ROI, Hough, line drawing), and returns the frame with detected lane lines.
*   `region_selection(image)`: Defines a polygonal region of interest and masks the input image to only consider edges within this area.
*   `hough_transform(image)`: Applies the Probabilistic Hough Transform to detect line segments from the edge-detected image.
*   `average_slope_intercept(lines)`: Processes the raw line segments from Hough Transform to determine the most representative left and right lane lines based on their slope and length.
*   `pixel_points(y1, y2, line)`: Converts the slope and intercept of a line into start and end pixel coordinates for drawing.
*   `lane_lines(image, lines)`: Uses `average_slope_intercept` and `pixel_points` to generate the final pixel coordinates for the left and right lane lines across the relevant vertical extent of the image.
*   `draw_lane_lines(image, lines, color, thickness)`: Draws the calculated lane lines onto the original image frame.
*   `process_video(test_video, output_video)`: The driver function that orchestrates the video processing using `moviepy`, applying `frame_processor` to each frame of the input video.
