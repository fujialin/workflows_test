# action runner config
由于github默认只支持x86_64环境，如遇到需其他架构（arm,windows,mac等）机器的场景，需要自己配置runner
```
runs-on: [self-hosted, Linux, ARM64]
```
## 本地启动镜像
```
docker run -dit --privileged=true --security-opt seccomp=unconfined \
        --name xxx_runner \
        -v /var/run/docker.sock:/var/run/docker.sock \
        -v /usr/bin/docker:/usr/bin/docker \
        -v /root/.docker/cli-plugins:/root/.docker/cli-plugins \
        ubuntu:20.04 bash
```
## 在镜像中创建配置用户
添加一个普通用户即可
## 下载客户端并配置runner
Settings->Actions->Runners->New self-hosted runner
```
Download
# Create a folder
$ mkdir actions-runner && cd actions-runner# Download the latest runner package
$ curl -o actions-runner-linux-arm64-2.296.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.296.0/actions-runner-linux-arm64-2.296.0.tar.gz
# Optional: Validate the hash
$ echo "954b55035056175c48f05733e457528a74ba8946124dc48d305d482844d7743d  actions-runner-linux-arm64-2.296.0.tar.gz" | shasum -a 256 -c
# Extract the installer
$ tar xzf ./actions-runner-linux-arm64-2.296.0.tar.gz
Configure
# Create the runner and start the configuration experience
$ ./config.sh --url https://github.com/xxx/xxx --token XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
# Last step, run it!
$ ./run.sh
Using your self-hosted runner
# Use this YAML in your workflow file for each job
runs-on: self-hosted
```
## 重新配置
Settings->Actions->Runners->Remove
```
Depending on if you have access to the runner machine, complete either of the following:

Remove and clean up machine (recommended)
If you have access to the machine, run the following shell command in the folder where you installed the self-hosted runner application. This command removes the runner from the repository, and also removes any configuration files and runner services on the machine.

// Remove the runner
$ ./config.sh remove --token XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Force runner removal
If you don't have access to the machine for a clean removal, you can click the button below to force remove the runner from your repository.
```
执行命令后会自动删除runner
or
```
Force remove this runner
```
注意强制删除了，本地也需要删除才能再次配置，主要是不好查询token,建议别强制删除，最好用命令删除
## 也可以在容器用户启动runner
切到工作目录
```
bin/Runner.Listener run --startuptype service
```

# 缺的库
graphviz

1.从 https://github.com/rockchip-linux/rknn-toolkit/raw/master/platform-tools/ntp/linux-aarch64/npu_transfer_proxy 下载文件；
2.mkdir -p /usr/rk_bins && cp -rf npu_transfer_proxy /usr/rk_bins && chmod +x /usr/rk_bins/npu_transfer_proxy;
3.升级新版本agent,删除旧的工程，重新创建新的工程；
4.重启设备后运行新工程；
5.以后按正常步骤创建工程即可。
