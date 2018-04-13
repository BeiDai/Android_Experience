## 调试经验

### 1、界面跳转失败

- 从开始界面使用 Handle.postDelay()方法，跳转到另一个界面失败
- 原因是新建的 activity 没有在Manifest中声名
- 即 activity android：name = ".MedicalBox"

### 2、
