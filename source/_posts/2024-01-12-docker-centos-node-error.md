---
title: 🚀 Docker解锁：解决在CentOS服务器上运行Node.js时的线程创建错误
date: 2024-01-12 16:45:28
tags:
- docker
- node
- centos
---


# docker运行node异常

在服务器中运行docker容器，镜像是一个Next.js的服务，需要运行node的服务，在本地macOS上运行docker容器是没问题，但在服务器(centOS系统)中却报错，错误信息如下：



# 报错信息

```shell
nodejs[2608]: ../src/node_platform.cc:61:std::unique_ptr<long unsigned int> node::WorkerThreadsTaskRunner::DelayedTaskScheduler::Start(): Assertion `(0) == (uv_thread_create(t.get(), start_thread, this))' failed.
 1: 0x7f620e2893cc node::Abort() [/lib/x86_64-linux-gnu/libnode.so.72]
 2: 0x7f620e28945b  [/lib/x86_64-linux-gnu/libnode.so.72]
 3: 0x7f620e30bde2 node::WorkerThreadsTaskRunner::WorkerThreadsTaskRunner(int) [/lib/x86_64-linux-gnu/libnode.so.72]
 4: 0x7f620e30bf16 node::NodePlatform::NodePlatform(int, v8::TracingController*) [/lib/x86_64-linux-gnu/libnode.so.72]
 5: 0x7f620e2542c8 node::InitializeOncePerProcess(int, char**) [/lib/x86_64-linux-gnu/libnode.so.72]
 6: 0x7f620e2544ac node::Start(int, char**) [/lib/x86_64-linux-gnu/libnode.so.72]
 7: 0x7f620d7c2d90  [/lib/x86_64-linux-gnu/libc.so.6]
 8: 0x7f620d7c2e40 __libc_start_main [/lib/x86_64-linux-gnu/libc.so.6]
 9: 0x5563adef00f5 _start [nodejs]
```
<!-- more -->
# 解决办法

从`StackExchange`上找到一个解决方案，尝试后解决。根据错误提示，是docker容器受到资源的限制，无法创建线程。

需要在docker run时添加`--security-opt seccomp=unconfined`参数，允许容器执行全部的系统调用，脚本如下：

```shell
docker run --security-opt seccomp=unconfined yourhub/yourimage:version
```

docker compose写法如下：

```yaml
security_opt:
    - seccomp=unconfined
```



详细可以参考官方文档：

[https://docs.docker.com/engine/security/seccomp/](https://docs.docker.com/engine/security/seccomp/)
