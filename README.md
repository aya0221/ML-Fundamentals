## ML-Fundamentals
Containing detailed explanations and practical code examples for key machine learning concepts, including Kalman Filters, Gradient Descent, and CNN architectures.

- [Kalman Filter Application in Object Tracking](#kalman-filter-application-in-object-tracking)
- [Gradient Gradient Descent Back Propagation SGD Application of Gradient](#gradient-gradient-descent-back-propagation-sgd-application-of-gradient)
- [Conv1D and Conv2D](#conv1d-and-conv2d)
- [Non-linear CNN](#non-linear-cnn)
- [Sources](#sources)


# Kalman Filter Application in Object Tracking

### Overview
I employed the Kalman Filter in object tracking tasks to enhance the tracking accuracy of balls in video sequences. This method improves tracking by using data from a CNN-based object detector, which, while effective, often includes some errors or 'noise.' These errors can arise due to variations in lighting, partial blockages of the object, or limitations of the detector itself. The Kalman Filter helps correct these inaccuracies by blending the imperfect measurements with predicted states from previous data, leading to more reliable tracking outcomes! Broad idea is it is propagating and updating Gaussians and updating their covariances.
<img src="https://github.com/aya0221/ML-Fundamentals/assets/69786640/2f370ff1-3541-4223-a7bd-29541c514e36" width="50%"> 
※1


### Components
- $x(t)$ State vector: Includes position $(x, y)$ and velocity $(vx, vy)$.
    Velocity is derived from changes in position over time, calculated as $$vx = (x_current - x_previous) / Δt$$
    $$vy = (y_current - y_previous) / Δt$$, where $Δt$ is the time interval between frames
-  $z(t)$ Measurement vector: Consists of the detected positions from the CNN, subject to measurement noise
-  $P(t|t-1)$ process covariance matrix
- $F$ State Transition matrix: Defines the linear relationship between the current state and the next state, factoring in time dynamics
-  $Q$ Process Noise Covariance: Quantifies the expected variability in the system dynamics, based on empirical data
- $R$ Measurement Noise Covariance: Represents the accuracy of the measurements, calculated from the variance of the CNN's outputs
- Control Matrix $B$ and Vector $u$: Generally set to zero, indicating no external forces affecting the motion of the object
- $H$ Measurement Matrix: Maps the state vector to the measurement vector, primarily focusing on the positional components and excluding velocity

### Mathematical Model
The Kalman Filter updates in two steps: **Prediction** and **Correction**. 
- Prediction step, the next state of the ball is estimated as:
$$x_predicted = x + vx * Δt$$
$$y_predicted = y + vy * Δt$$

- Correction step adjusts this prediction based on the new measurement:
$$K = P^- H^T (H P^- H^T + R)^{-1}$$

$$x_corrected = x_predicted + K(z - H * x_predicted)$$
$$P_corrected = (I - K H) P^-$$, where $K$ is the Kalman Gain, $P^-$ is the predicted covariance, $I$ is the identity matrix, and $z$ is the actual measurement from the object detector

<img src="https://github.com/aya0221/ML-Fundamentals/assets/69786640/df49494d-4ae1-4587-ade9-282ec67b5f32" width="40%"> 
※2

### Utilization of Velocity
Velocity is critical for predicting the future position of the ball, particularly in cases of occlusion or when the ball moves out of the camera frame. By estimating velocity, the Kalman Filter can project the ball's trajectory, enhancing tracking continuity and robustness under varying conditions.

### How Kalman Filter is useful for real-time object tracking
The implementation of the Kalman Filter in this project demonstrates its efficacy in combining real-time object detection with predictive tracking, providing a robust solution for tracking objects in motion within noisy environments





# Gradient, Gradient Descent, Back Propagation, SGD, Application of Gradient
- **Gradient** is a vector of partial derivatives that *points in the direction of the greatest increase of a function*. In ML, we use the gradient to update the weights of models, *moving in the direction that most reduces the loss*.

- **Gradient Descent** is an optimization algorithm used to minimize a function by iteratively moving towards the steepest descent as defined by the *negative* of the gradient. Goal is to find the model parameters that minimize a loss function.
let parameter p:
$$p = p-learning rate * ∂/∂p = p-α * ∂/∂p$$

- **Back Propagation** is used to calculate the gradient needed in the gradient descent step of neural network training by computing the gradient of the loss function with respect to each weight by the **chain rule**, working backward from the output layer to the input layer.

- **Difference Between SGD and Gradient Descent**:
    - **SGD** updates the parameters using only a small subset of the data which can lead to faster convergence on large datasets
    - **Batch Gradient Descent** uses the entire dataset to perform one update at a time

- **Application of Gradient**
During training, repeatedly adjusting the parameters of the model, using either the whole dataset (batch) or subsets of it (mini-batches), to minimize the loss function over multiple iterations or epochs

# Conv1D and Conv2D
- **Conv1D**: for 1D data, e.g., time-series, audio data - *where the layer will learn from patterns occurring over time*.
- **Conv2D** for 2D data, e.g., images - *where the layers help learn features from the input by applying filters that capture spatial hierarchies*, (identifying simple edges in early layers and complex features like textures in deeper layers).


# Non-linear CNN
A Non-linear CNN incorporates non-linear activation functions like ReLU (Rectified Linear Unit) to introduce non-linear properties into the model, allowing it to learn more complex patterns. Without non-linearities, CNNs would behave just like a single linear classifier.



# Sources
※1:[Sort and Deep-SORT Based Multi-Object Tracking for Mobile Robotics: Evaluation with New Data Association Metrics](https://www.researchgate.net/publication/358134782_Sort_and_Deep-SORT_Based_Multi-Object_Tracking_for_Mobile_Robotics_Evaluation_with_New_Data_Association_Metrics)
※2: [Articles / Artificial Intelligence / Computer vision](https://www.codeproject.com/Articles/865935/Object-Tracking-Kalman-Filter-with-Ease)
