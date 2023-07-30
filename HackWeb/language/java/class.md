类
========================================

生命周期
----------------------------------------
整体来说，Java中类的生命周期如下：加载 (Loading) -> [ 连接 (Linking) : 验证 (Verification) -> 准备 (Perparation) -> 解析 (Resolutin) ] -> 初始化 (Initialization) -> 使用 (Using) -> 卸载 (Unloading) 。

加载过程分为三步：

- 通过全限定类名来获取定义此类的二进制字节流
- 将字节流所代表的静态存储结构转化为方法区的运行时数据结构
- 在内存中生成代表这个类的 ``java.lang.Class`` 对象，作为方法区这个类的各种数据的访问入口

验证阶段主要用于确保 Class 文件的字节流符合当前虚拟机的要求，分为几步：

- 判断文件格式：是否以 ``0xCAFEBABE`` 开始，主次版本号是否在处理范围内
- 元数据验证
- 字节码验证
- 符号引用验证