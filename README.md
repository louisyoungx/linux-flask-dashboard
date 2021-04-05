<h1 align="center">
  <a href="https://gitee.com/louisyoung1/projects">
    <img src="https://portrait.gitee.com/uploads/avatars/user/2529/7589523_louisyoung1_1597595544.png"/>
  </a>
</h1>

<p align="center">
  <sub>v1.0</sub><br/>
  <small>一个简单 & 轻量的 web 仪表盘 for linux 系统</small>
</p>

<p align="center">
  <small>
    <a href="#">Demo</a> &nbsp;|&nbsp;
    <a href="https://gitee.com/louisyoung1/linux-flask-dashboard/blob/master/README.md">
      文档
    </a> &nbsp;|&nbsp;
    <a href="https://gitee.com/louisyoung1/linux-flask-dashboard/blob/master/README.en.md">
      English
    </a>
  </small>
</p>



<br/>

## 一、亮点
* **轻量** ------ 占用在400KB以下
* **美观** ------ 极简主义，漂亮的仪表盘
* **简单** ------ 上手简洁
* **多样** ------ 选择使用 Python, Docker的版本

## 二、安装

### 第一步
```sh
## 1. clone the repo
git clone https://gitee.com/louisyoung1/linux-flask-dashboard.git

## 2. go to the cloned directory
cd linux-flask-dashboard

## 3. use pip to install requirements
python3 -m pip install -r requirements.txt

```
或者，如果你喜欢手动下载:

```sh
## 1. Download the .zip & unzipped dir
curl -LOk https://gitee.com/louisyoung1/linux-flask-dashboard/repository/archive/master.zip && unzip master.zip

## 2. go to the downloaded directory
cd linux-flask-dashboard

## 3. use pip to install requirements
python3 -m pip install -r requirements.txt

##(若第三步CentOS系统安装psutil报错，可尝试 yum -y install python3-devel)
##(若CentOS启动后打不开页面，请检查firewalld防火墙配置！)

```

### 第二步

详见 Linux Flask Dashboard 支持的版本 :

* [Python](#使用-Python)
* [Docker](#使用-Docker)

#### 使用 Python
```sh
## 1. Start the server (on port 12005 by default; may require sudo).
python index.py

## 2. Open your browser to visit [http://0.0.0.0:12005]  (on port 12005 by default; may require sudo)
# 浏览器访问 http://0.0.0.0:12005
```

#### 使用 Docker
确保你已经启用了Docker，你可以用一行命令安装Docker
```sh
## make sure you have install docker in your server
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

### 方法一（使用源代码创建Docker镜像）
```sh
## 1. go to the cloned directory
cd linux-flask-dashboard

## 2. build docker image
sudo docker build -t linux-flask-dashboard .

## 3. start docker container
docker run -itd -p 12005:12005 --restart=always --name flask-dash-test linux-flask-dashboard:latest

## 2. Open your browser to visit [http://0.0.0.0:12005]  (on port 12005 by default; may require sudo)
# 浏览器访问 http://0.0.0.0:12005
```

### 方法二（从Docker Hub拉取镜像）
```sh
## 1. pull from docker hub
docker pull louisyoung1/linux-flask-dashboard

## 2. start docker container
docker run -itd -p 12005:12005 --restart=always --name flask-dash-test linux-flask-dashboard:latest

## 3. Open your browser to visit [http://0.0.0.0:12005]  (on port 12005 by default; may require sudo)
# 浏览器访问 http://0.0.0.0:12005
```

## 三、快速上手

开启服务:<br>
`$ python3 main.py`

启用一个代理:<br>
`$ python3 main.py -a --register-to [http|https]://[host]:[port] --register-as my-agent-node`

这将在 *代理模式* 下启动服务，并尝试将节点注册到 `--register-to` 选项所指向的主节点。
代理节点将分别在 `-p/--port` 和 `-b/--bind` 指定的主机和端口上设置RPC服务器，以替代web服务器。
主psdash节点(提供HTTP服务)将提供一个注册节点列表，可以在这些节点之间进行切换。

可用的命令行参数:
```
$ python3 main.py --help
usage: psdash [-h] [-l path] [-b host] [-p port] [-d] [-a]
              [--register-to host:port] [--register-as name]

psdash [version] - system information web dashboard

optional arguments:
  -h, --help            show this help message and exit
  -l path, --log path   log files to make available for psdash. Patterns (e.g.
                        /var/log/**/*.log) are supported. This option can be
                        used multiple times.
  -b host, --bind host  host to bind to. Defaults to 0.0.0.0 (all interfaces).
  -p port, --port port  port to listen on. Defaults to 5000.
  -d, --debug           enables debug mode.
  -a, --agent           Enables agent mode. This launches a RPC server, using
                        zerorpc, on given bind host and port.
  --register-to host:port
                        The psdash node running in web mode to register this
                        agent to on start up. e.g 10.0.1.22:5000
  --register-as name    The name to register as. (This will default to the
                        node's hostname)
```

## 四、设置

Linux Flask Dashboard 使用了Flask提供配置。
环境变量' PSDASH_CONFIG '所指向的配置文件将在启动时读取。<br>
e.g: `$ PSDASH_CONFIG=/home/user/config.py psdash`

除了特定的 [Flask内置可选设置](http://flask.pocoo.org/docs/config/#builtin-configuration-values) 之外，Linux Flask Dashboard 也提供了额外的可选设置:

| 名称 | 描述 |
| ---- | ----------- |
| `PSDASH_AUTH_USERNAME` | 登录验证的用户名。当这个值和`PSDASH_AUTH_PASSWORD` 同时设置后，访问将需要使用登录验证 |
| `PSDASH_AUTH_PASSWORD` | 登录验证的密码 |
| `PSDASH_ALLOWED_REMOTE_ADDRESSES` | 如设置，则只允许使用该ip地址访问。地址之间用逗号分隔，例如: `PSDASH_ALLOWED_REMOTE_ADDRESSES = "10.0.0.2, 192.29.20.2"` |
| `PSDASH_URL_PREFIX` | 这可以用来从非根位置提供服务 例如: `PSDASH_URL_PREFIX = "/dash"` 将使服务页从 /dash 启动 |
| `PSDASH_LOG_LEVEL` | 设置日志级别 (传入 `logging.basicConfig`). *默认设置为 `logging.INFO`*. |
| `PSDASH_LOG_LEVEL` | 设置日志格式 (传入 `logging.basicConfig`). *默认设置为 `%(levelname)s | %(name)s | %(message)s`*. |
| `PSDASH_NODES` | 要在启动时注册的代理节点列表(每个节点一个字典)。如： `[{'name': 'mywebnode', 'host': '10.0.0.2', 'port': 5000}]` |
| `PSDASH_NET_IO_COUNTER_INTERVAL` | 更新用于计算网络流量的计数器的时间间隔(s)。 *默认为3秒*. |
| `PSDASH_LOGS_INTERVAL` | 重新应用日志模式以确保应用文件系统更改(创建或删除日志文件)的间隔(以秒为单位)。*默认为60 *。|
| `PSDASH_REGISTER_INTERVAL` | 将代理注册到主机节点的间隔(以秒为单位)。定期进行以便能够确定任何节点是否已经消失以及在什么时候消失。*默认为60 * |
| `PSDASH_LOGS` | 记录在启动时应用的模式。 例如： `['/var/log/*.log']`. 要使用命令行覆盖这个选项，请使用 `-l/--log` 参数选项。 |
| `PSDASH_REGISTER_TO` | 在代理模式下运行时，它用于设置将代理节点注册到哪个psdash节点。如 `http://10.0.20.2:5000` |
| `PSDASH_REGISTER_AS` | 在代理模式下运行时，它用于设置要注册到主机节点的名称，该节点由 `PSDASH_REGISTER_TO`指定. |
| `PSDASH_HTTPS_KEYFILE` | 用于启用以HTTPS方式启动web服务器的SSL密钥文件的路径。如 `/home/user/private.key`
| `PSDASH_HTTPS_CERTFILE` | 用于启用以HTTPS方式启动web服务器的SSL证书文件路径。如 `/home/user/certificate.crt`
| `PSDASH_ENVIRON_WHITELIST` | 如果设置了，则只会显示此列表中的env变量的值。如`['HOME']`

## 五、预览

未完待续······

## 六、技术支持

有技术难题，在 [Louis-House](https://www.louisyoung.work/Contact)有我的联系方式

## 七、安全

**强烈建议 **在安全的环境下安装**[Linux Flask Dashboard](https://gitee.com/louisyoung1/linux-flask-dashboard)**

**[Linux Flask Dashboard](https://gitee.com/louisyoung1/linux-flask-dashboard)** 仅提供简单安全性与身份验证特性。

## 八、许可

The GPL License (**[GPL-3.0](https://gitee.com/louisyoung1/linux-flask-dashboard/blob/master/LICENSE)**)