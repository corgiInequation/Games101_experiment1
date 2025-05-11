# Games101 Experiment 1

## Experiment Content

- Understand the use of the MVP matrix and manually construct the MVP matrix based on learned knowledge.

- Main code is as follows:

- ```c++
  Eigen::Matrix4f get_model_matrix(float rotation_angle)
  {
      Eigen::Matrix4f model = Eigen::Matrix4f::Identity();

      // TODO: Implement this function
      // Create the model matrix for rotating the triangle around the Z axis.
      // Then return it.
      Eigen::Matrix4f rotate;
      float radian = rotation_angle/180.0*MY_PI;
      rotate << cos(radian), -1*sin(radian), 0, 0,
                sin(radian), cos(radian), 0, 0,
                0, 0, 1, 0,
                0, 0, 0, 1; // Simply implements rotation matrix around Z-axis
      model = rotate * model;
      return model;
  }
  ```

- ```c++
  Eigen::Matrix4f get_projection_matrix(float eye_fov, float aspect_ratio,
      float zNear, float zFar)
  {
      // Students will implement this function
      // TODO: Implement this function
      // Create the projection matrix for the given parameters.
      // Then return it.
      Eigen::Matrix4f projection = Eigen::Matrix4f::Identity();
      Eigen::Matrix4f P2O = Eigen::Matrix4f::Identity(); // Matrix that converts perspective projection to orthographic projection
      P2O << zNear, 0, 0, 0,
             0, zNear, 0, 0,
             0, 0, zNear + zFar, (-1) * zFar * zNear,
             0, 0, 1, 0; // Perspective to orthographic conversion matrix
      float halfEyeAngleRadian = eye_fov / 2.0 / 180.0 * MY_PI;
      float t = zNear * std::tan(halfEyeAngleRadian); // Top (maximum y-axis value)
      float r = t * aspect_ratio; // Right (maximum x-axis value)
      float l = (-1) * r; // Left (minimum x-axis value)
      float b = (-1) * t; // Bottom (minimum y-axis value)
      Eigen::Matrix4f ortho1 = Eigen::Matrix4f::Identity();
      ortho1 << 2 / (r - l), 0, 0, 0,
                0, 2 / (t - b), 0, 0,
                0, 0, 2 / (zNear - zFar), 0,
                0, 0, 0, 1; // Scale to a standard cube with length 2
      Eigen::Matrix4f ortho2 = Eigen::Matrix4f::Identity();
      ortho2 << 1, 0, 0, (-1) * (r + l) / 2,
                0, 1, 0, (-1) * (t + b) / 2,
                0, 0, 1, (-1) * (zNear + zFar) / 2,
                0, 0, 0, 1; // Translate the cuboid center to the origin
      Eigen::Matrix4f Matrix_ortho = ortho1 * ortho2;
      projection = Matrix_ortho * P2O;
      return projection;
  }
  ```

## Experiment Results

- ![image-20250511123707212](https://github.com/corgiInequation/Games101_useOfMvpMatrix/blob/main/triangle_1.png)

- After pressing A

- ![image-20250511123728176](https://github.com/corgiInequation/Games101_useOfMvpMatrix/blob/main/triangle_2.png)

- After pressing D

- ![image-20250511123749920](https://github.com/corgiInequation/Games101_useOfMvpMatrix/blob/main/triangle_3.png)
