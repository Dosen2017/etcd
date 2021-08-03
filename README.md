# etcd简单使用
#### 下载地址
  https://github.com/etcd-io/etcd/releases/download/v3.5.0/etcd-v3.5.0-linux-amd64.tar.gz <br/>
  可以通过 : https://github.com/etcd-io/etcd/releases  找到最新的稳定版本下载
  
#### 解压安装
  解压：tar -xzvf etcd-v3.5.0-linux-amd64.tar.gz <br/>
  安装：mv etcd-v3.5.0-linux-amd64 /opt/etcd

#### 简单启动
  cd /opt/etcd <br/>
  ./etcd

#### 创建systemd服务
  ###### 1.设定etcd配置文件
    mkdir -p /var/lib/etcd/  
    mkdir -p /opt/etcd/config/
  ###### 2.创建etcd配置文件
    cat <<EOF | sudo tee /opt/etcd/config/etcd.conf
    #节点名称
    ETCD_NAME=$(hostname -s)
    #数据存放位置
    ETCD_DATA_DIR=/var/lib/etcd
    EOF
  ###### 3.创建systemd配置文件
    cat <<EOF | sudo tee /etc/systemd/system/etcd.service
    [Unit]
    Description=Etcd Server
    Documentation=https://github.com/coreos/etcd
    After=network.target

    [Service]
    User=root
    Type=notify
    EnvironmentFile=-/opt/etcd/config/etcd.conf
    ExecStart=/opt/etcd/etcd
    Restart=on-failure
    RestartSec=10s
    LimitNOFILE=40000

    [Install]
    WantedBy=multi-user.target
    EOF
    
  ##### 4.启动etcd
    systemctl daemon-reload && systemctl enable etcd && systemctl start etcd <br/>
    start/stop/restart 启动/停止/重启
    
#### 基本操作
  ###### 查看版本(服务端)：
    /opt/etcd/etcd --version
  
  ###### 查看常用命令(客户端)：
    /opt/etcd/etcdctl -h
  ###### 添加查找删除:
    /opt/etcd/etcdctl put /test/aaa bbb 
    /opt/etcd/etcdctl get /test/aaa
    /opt/etcd/etcdctl del /test/aaa
    
    **windows 查找前缀为user.rpc的方法**
    .\etcdctl.exe get user.rpc --prefix
    
    
