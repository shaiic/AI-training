## 教学环境的Docker构建
### 如何使用

在`Env-Build`文件夹中，通过修改 `environment.yml` 文件可以增加删除相关python库，通过修改`Dockerfile`可以修改其它需要的安装依赖库，便于构建本地docker image。

> 目前使用的Base image来自`ddebby/ai:v0.1.0`，精简版的`ubuntu`操作系统，集成了conda环境和python3.7

执行如下命令创建`ds-docker`的image，并使用该image构建一个环境，启动两个服务：

```
docker build -t ds-docker .
docker run --name ds01 -d -p 8443:8443 -p 8888:8888 -v $(pwd)/data:/data -v $(pwd)/code:/code --rm ds-docker
```

VSCode: http://localhost:8443 with a token of `******`

JupyterLab: http://localhost:8888 with a token of `********`

### 目前包含的相关库

1. Conda for Python dependencies
2. VSCode for full featured development（by [Coder's](https://coder.com/)）
3. JupyerLab for quick protyping
4. tensorflow 2.0.0-alpha0

### 其它

如果需要安装VSCode的扩展或修改VSCode的配置，相关信息将被持久化在`/data`文件夹中。

相关代码和`notebook`请放在`/code`文件夹下；

### TO-DO

1. 收集更多的环境依赖



