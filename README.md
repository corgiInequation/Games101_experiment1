# Games101 experiment 1

## 实验内容

- 了解MVP矩阵的使用，根据自己所学知识，去手动构造MVP矩阵

- 以下为主要代码

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
                0, 0, 0, 1;//单纯实现了关于z轴的旋转矩阵
      model = rotate * model; 
      return model;
  }
  ```

- ```c++
  igen::Matrix4f get_projection_matrix(float eye_fov, float aspect_ratio,
      float zNear, float zFar)
  {
      // Students will implement this function
      // TODO: Implement this function
      // Create the projection matrix for the given parameters.
      // Then return it.
      Eigen::Matrix4f projection = Eigen::Matrix4f::Identity();
      Eigen::Matrix4f P2O = Eigen::Matrix4f::Identity();//将透视投影转换为正交投影的矩阵
      P2O<<zNear, 0, 0, 0,
      0, zNear, 0, 0,
      0, 0, zNear+zFar,(-1)*zFar*zNear,
      0, 0, 1, 0;// 进行透视投影转化为正交投影的矩阵
      float halfEyeAngelRadian = eye_fov/2.0/180.0*MY_PI;
      float t = zNear*std::tan(halfEyeAngelRadian);//top y轴的最高点
      float r=t*aspect_ratio;//right x轴的最大值
      float l=(-1)*r;//left x轴最小值
      float b=(-1)*t;//bottom y轴的最大值
      Eigen::Matrix4f ortho1=Eigen::Matrix4f::Identity();
      ortho1<<2/(r-l),0,0,0,
      0,2/(t-b),0,0,
      0,0,2/(zNear-zFar),0,
      0,0,0,1;//进行一定的缩放使之成为一个标准的长度为2的正方体
      Eigen::Matrix4f ortho2 = Eigen::Matrix4f::Identity();
      ortho2<<1,0,0,(-1)*(r+l)/2,
      0,1,0,(-1)*(t+b)/2,
      0,0,1,(-1)*(zNear+zFar)/2,
      0,0,0,1;// 把一个长方体的中心移动到原点
      Eigen::Matrix4f Matrix_ortho = ortho1 * ortho2;
      projection = Matrix_ortho * P2O;
      return projection;
  }
  ```



## 实验结果

- <img src="C:\Users\i love china\AppData\Roaming\Typora\typora-user-images\image-20250511123707212.png" alt="image-20250511123707212" style="zoom:50%;" />

- 按下A之后

- <img src="C:\Users\i love china\AppData\Roaming\Typora\typora-user-images\image-20250511123728176.png" alt="image-20250511123728176" style="zoom:50%;" />

- 按下D之后

- <img src="C:\Users\i love china\AppData\Roaming\Typora\typora-user-images\image-20250511123749920.png" alt="image-20250511123749920" style="zoom:50%;" />

  