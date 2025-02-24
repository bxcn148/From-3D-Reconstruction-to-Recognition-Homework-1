Download link :https://programming.engineering/product/from-3d-reconstruction-to-recognition-homework-1/


# From-3D-Reconstruction-to-Recognition-Homework-1
From 3D Reconstruction to Recognition Homework #1
Projective Geometry Problems [20 points]

In this question, we will examine properties of projective transformations. We de ne a camera coordinate system, which is only rotated and translated from a world coordinate system.

Prove that parallel lines in the world reference system are still parallel in the camera reference system. [4 points]

Consider a unit square pqrs in the world reference system where p; q; r; and s are points. Will the same square in the camera reference system always have unit area? Prove or provide a counterexample. [4 points]

Now let’s consider a ne transformations, which are any transformations that preserve par-allelism. A ne transformations include not only rotations and translations, but also scaling and shearing. Given some vector p, an a ne transformation is de ned as

A(p) = Mp + b

where M is an invertible matrix. Prove that under any a ne transformation, the ratio of parallel line segments is invariant, but the ratio of non-parallel line segments is not invariant. [6 points]

You have explored whether these three properties hold for a ne transformations. Do these properties hold under any projective transformation? Justify brie y in one or two sentences (no proof needed). [6 points]

A ne Camera Calibration (35 points)

In this question, we will perform a ne camera calibration using two di erent images of a calibration grid. First, you will nd correspondences between the corners of the calibration grids and the 3D scene coordinates. Next, you will solve for the camera parameters.


It was shown in class that a perspective camera can be modeled using a 3 4 matrix:

y

= p21

p22

p23

p24

2

Y

3

(1)

2w3

2p31

p32

p33

p343

6

X

Z 7

4

x

5

p11

p12

p13

p14

56

7

4

1

4

5

which means that the image at point (X; Y; Z) in the scene has pixel coordinates (x=w; y=w). The 3 4 matrix can be factorized into intrinsic and extrinsic parameters.

An a ne camera is a special case of this model in which rays joining a point in the scene to its projection on the image plane are parallel. Examples of a ne cameras include orthographic projection and weakly perspective projection. An a ne camera can be modeled as:

y

= p21

p22

p23

p24

2

Y

3

(2)

21

3

2 0

0

0

1 3

6

X

Z 7

x

4

p11

p12

p13

p14

56

7

4 5

1

4

5

which gives the relation between a scene point (X; Y; Z) and its image (x; y). The di erence is that the bottom row of the matrix is [ 0 0 0 1 ], so there are fewer parameters we need to calibrate. More importantly, there is no division required (the homogeneous coordinate is 1) which means this is a linear model. This makes the a ne model much simpler to work with mathematically – at the cost of losing some accuracy. The a ne model is used as an approximation of the perspective model when the loss of accuracy can be tolerated, or to reduce the number of parameters being modeled. Calibration of an a ne camera involves estimating the 8 unknown entries of the matrix in Eq. 2 (This matrix can also be factorized into intrinsics and extrinsics, but that is outside the scope of this homework). Factorization is accomplished by having the camera observe a calibration pattern with easy-to-detect corners.

Scene Coordinate System

The calibration pattern used is shown in Figure 1, which has a 6 6 grid of squares. Each square is 50mm 50mm. The separation between adjacent squares is 30mm, so the entire grid is 450mm 450mm. For calibration, images of the pattern at two di erent positions were captured. These images are shown in Fig. 1 and can be downloaded from the course website. For the second image, the calibration pattern has been moved back (along its normal) from the rest position by 150mm.

We will choose the origin of our 3D coordinate system to be the top left corner of the calibration pattern in the rest position. The X-axis runs left to right parallel to the rows of squares. The Y -axis runs top to bottom parallel to the columns of squares. We will work in units of millimeters. All the square corners from the rst position corresponds to Z = 0. The second position of the calibration grid corresponds to Z = 150. The top left corner in the rst image has 3D scene coordinates (0; 0; 0) and the bottom right corner in the second image has 3D scene coordinates (450; 450; 150). This scene coordinate system is labeled in Fig. 1.

Given correspondences for the calibrating grid, solve for the camera parameters using Eq. 2. Note that each measurement (xi; yi) $ (Xi; Yi; Zi) yields two linear equations for the 8 unknown camera parameters. Given N corner measurements, we have 2N equations and 8 unknowns. Using the given corner correspondences as inputs, complete the method compute camera matrix(). You will construct a linear system of equations and solve for the camera parameters to minimize the least-squares error. After doing so, you will return the 3 4 a ne camera matrix composed of these computed camera parameters. In your written


Image formation in an a ne camera. Points are projected via parallel rays onto the image plane


(b) Image of calibration grid at Z=0 (c) Image of calibration grid at Z=150

Figure 1: A ne camera: image formation and calibration images.

report, submit a brief description of your approach as well as the camera matrix that you compute. [15 points]

After nding the calibrated camera matrix, you will compute the RMS error between the given N image corner coordinates and N corresponding calculated corner locations in rms error().

Recall that

q

X

RMStotal = ((x x0 )2 + (y y0 )2)=N

Please submit the RMS error for the camera matrix that you found in part (a). [15 points]

Could you calibrate the matrix with only one checkerboard image? Explain brie y in one or two sentences. [5 points]

Single View Geometry (45 points)

In this question, we will estimate camera parameters from a single view and leverage the projective nature of cameras to nd both the camera center and focal length from vanishing points present in the scene above.


(a) Image 1 (1.jpg) with marked pixels (b) Image 2 (2.jpg) with marked pixels

Figure 2: Marked pixels in images taken from di erent viewpoints.

In Figure 2, we have identi ed a set of pixels to compute vanishing points in each image. Please complete compute vanishing point(), which takes in these two pairs of points on parallel lines to nd the vanishing point. You can assume that the camera has zero skew and square pixels, with no distortion. [5 points]

Using three vanishing points, we can compute the intrinsic camera matrix used to take the image. Do so in compute K from vanishing points(). [10 points]

Is it possible to compute the camera intrinsic matrix for any set of vanishing points? Similarly, is three vanishing points the minimum required to compute the intrinsic camera matrix? Justify your answer. [5 points]

The method used to obtain vanishing points is approximate and prone to noise. Discuss approaches to re ne this process. [5 points]

This process gives the camera internal matrix under the speci ed constraints. For the re-mainder of the computations, use the following internal camera matrix:

3

2448

0

1253

K = 4

0

2438

986

5

0

0

1

Use the vanishing lines we provide in the starter code to verify numerically that the ground plane is orthogonal to the plane front face of the box in the image. Fill out the method compute angle between planes() and include a brief description of your solution and your computed angle in your report. [10 points]

Assume the camera rotates but no translation takes place. Assume the internal camera parameters remain unchanged. An Image 2 of the same scene is taken. Use vanishing points to estimate the rotation matrix between when the camera took Image 1 and Image 2. Fill out the method compute rotation matrix between cameras() and submit a brief description of your approach and your results in the written report. [10 points]
