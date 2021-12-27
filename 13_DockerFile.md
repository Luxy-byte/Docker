# DockerFile

构建步骤：

1. 编写一个dockerfile文件
2. docker build 构建成为一个镜像
3. docker run 运行镜像容器
4. docker push 发布镜像（docker hub 或 阿里云）

## DockerFile构建过程

基础知识：

1. 每个保留关键字（指令）都是必须是大写字母
2. 执行从上到下顺序执行
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像层

![DockerFile](C:\Users\alienware\Desktop\Docker_Learn\DockerFile.png)

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行的产品

DockerContainer：容器就是镜像运行起来提供服务的

