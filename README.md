It begins by iterating through each extracted frame from the image paths. For each frame, it
converts the image to grayscale and then applies the cv2.findChessboardCorners() function to
detect corners based on the specified pattern size.
The cv2.findChessboardCorners() function employs various technical processes to detect the inter-
nal corners of a chessboard pattern in an image. It analyzes image gradients to identify regions of
sharp intensity changes, utilizes corner response functions like Harris or Shi-Tomasi detectors to
score potential corner points based on gradient variations, and applies local maximum suppression
to select the most significant corners. Pattern matching is then performed to validate corner posi-
tions against the expected pattern size, with iterative refinement techniques enhancing accuracy.
Step-by-step explanation of each stage of the calibration pipeline:
Detection of Calibration Pattern: In this stage, the calibration pattern’s keypoints or corners are
detected in each acquired image. This is typically done using feature detection algorithms like
SIFT. These algorithms identify distinctive points or regions in the image that can be reliably
matched between different images.

![image](https://github.com/saidineshgelam/Camera-calibration-using-a-checker-board/assets/144295692/dd23ca1f-8568-4cce-84be-7b906d82e321)


Matching Keypoints: Once the keypoints are detected in each image, the next step is to match
the keypoints between pairs of images. This process establishes correspondences between the same
physical points in different images, allowing for accurate calibration.
Estimation of Camera Parameters: Using the matched keypoints, the camera’s intrinsic and ex-
trinsic parameters are estimated. The intrinsic parameters include focal length, principal point,
and lens distortion coefficients, while the extrinsic parameters describe the camera’s position and
orientation in the scene.
The cv2.calibrateCamera() function estimates the intrinsic camera parameters (focal length, optical
center) and distortion coefficients by analyzing detected 3D object points and their corresponding
2D image points. It optimizes these parameters to minimize the difference between observed and
projected image points, providing the camera matrix (mtx), distortion coefficients (dist), rotation
vectors (rvecs), and translation vectors (tvecs).
Refinement of Parameters: After the initial estimation, an optimization process may be employed
to refine the camera parameters further. This optimization aims to minimize the reprojection error,
which is the discrepancy between the observed keypoints and their corresponding projected points
calculated using the estimated parameters.
Validation and Evaluation: Once the calibration is complete, the accuracy of the estimated pa-
rameters is validated. This validation may involve assessing the reprojection error, comparing the
projected keypoints with manually annotated ground truth points.
The cv2.projectPoints() function projects the 3D object points (objp) into the 2D image plane using
the estimated rotation vectors (rvecs[-1]) and translation vectors (tvecs[-1]), along with the intrinsic
camera parameters (mtx) and distortion coefficients (dist). This process calculates the correspond-
ing 2D image points (imgpoints2), providing a mapping of the object’s spatial coordinates onto the
camera’s image plane.
cv2.norm() function calculates the Euclidean distance between the detected corners (corners) and
the corresponding reprojection points (imgpoints2). The distance is computed using the L2 norm
6(cv2.NORM_L2). Finally, the average distance is computed by dividing the total distance by the
number of reprojection points (len(imgpoints2)), yielding the reprojection error. This error metric
quantifies the accuracy of the camera calibration process by measuring the deviation between the
observed and projected image points.

![image](https://github.com/saidineshgelam/Camera-calibration-using-a-checker-board/assets/144295692/9900b900-1eef-4f55-aebc-cd4fa72b3fa6)

The reprojection error is a critical metric in camera calibration as it quantifies the accuracy of
the calibration process. It represents the discrepancy between the observed image points (detected
corners) and the projected image points obtained by back-projecting 3D object points onto the
image plane using the calibrated camera parameters. A low reprojection error indicates that the
calibration accurately models the camera’s intrinsic and extrinsic parameters, resulting in precise
mapping between 3D world coordinates and 2D image coordinates. Conversely, a high reprojection
error suggests inaccuracies in the calibration, which can lead to errors in subsequent computer
7vision tasks such as 3D reconstruction, object tracking, and augmented reality.

![image](https://github.com/saidineshgelam/Camera-calibration-using-a-checker-board/assets/144295692/57a5794a-7172-497b-b583-f33fdcbdc27b)

First, it loads the first 5 extracted frames from the list of frame paths and reads
its dimensions. Then, it undistorts the image using the camera matrix (mtx), distortion coeffi-
cients (dist), and optional rectification transformation (None) obtained during calibration. Next,
it draws the detected chessboard corners on the original image using cv2.drawChessboardCorners()
before calibration and draws the corresponding corners after calibration on the undistorted image.
Finally, it displays both the original and undistorted images side by side using matplotlib.pyplot.
