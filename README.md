# Control-UR-Robotic-Arm-to-Visually-Capture-Specific-Fruits-With-HSV-Target-Detection

## 基于HSV颜色模型识别不同水果，控制UR机械臂抓取运动中不同水果将其分类摆放

### 环境配置

- 首先要将gxipy放到anaconda文件夹里对应环境的lib下，要保证这个环境下有numpy，PIL等基本的包

  - 或者通过

    ```python
    import sys
    sys.path.append(“gxipy的路径”)
    ```

    把gxipy的地址导入

- 安装UR机械臂驱动程序

  ```
  pip install ur_rtde
  ```

- 设置IP，建立于机械臂的通信

### 多目标识别决策树

测试中出现小橘子，小番茄，桂圆三种水果

1. 通过HSV颜色空间分离出橙黄色的物体（小橘子和小桂圆颜色相近），排除掉红色的小番茄
2. 通过外接圆半径区分小橘子和小桂圆（橘子半径是桂圆两倍以上）
3. 通过HSV识别红色分离出小番茄

因此这个策略会实现一下效果

- 视野中如果出现小橘子（即使同时存在小番茄，桂圆），返回最大的橘子的坐标与代表橘子类别的FLAG
- 如果视野中没有小橘子，则依次找桂圆和番茄，如果什么都没有返回None
