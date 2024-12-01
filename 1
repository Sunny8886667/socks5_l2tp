#!/bin/bash

# 检查是否以root权限运行
if [ "$EUID" -ne 0 ]; then
  echo "请以root权限运行此脚本。"
  exit 1
fi

# 提示用户输入L2TP配置信息
echo "请输入L2TP服务器的IP地址:"
read -r L2TP_REMOTE_IP
echo "请输入L2TP用户名:"
read -r L2TP_USERNAME
echo "请输入L2TP密码:"
read -r L2TP_PASSWORD
echo "请输入要设置的SOCKS5监听端口列表 (用逗号分隔，例如: 1080,1081):"
read -r SOCKS5_PORTS

# 分割监听端口
IFS=',' read -r -a PORT_ARRAY <<< "$SOCKS5_PORTS"

# 定义L2TP接口前缀
L2TP_INTERFACE_PREFIX="ppp"

# 安装必要的软件包
if ! command -v socat &> /dev/null || ! command -v xl2tpd &> /dev/null; then
  echo "正在安装必要的软件包..."
  apt-get update
  apt-get install -y socat xl2tpd iptables
fi

# 配置L2TP
cat <<EOF > /etc/xl2tpd/xl2tpd.conf
[lac l2tp_connection]
lns = $L2TP_REMOTE_IP
ppp debug = yes
pppoptfile = /etc/ppp/options.l2tpd.client
length bit = yes
EOF

cat <<EOF > /etc/ppp/options.l2tpd.client
ipcp-accept-local
ipcp-accept-remote
refuse-eap
require-chap
noccp
noauth
idle 1800
mtu 1410
mru 1410
defaultroute
usepeerdns
connect-delay 5000
name $L2TP_USERNAME
password $L2TP_PASSWORD
EOF

# 启动L2TP连接
echo "启动L2TP连接..."
service xl2tpd restart
sleep 2

# 启动多个L2TP连接和SOCKS5代理
for i in "${!PORT_ARRAY[@]}"; do
  PORT=${PORT_ARRAY[i]}
  L2TP_INTERFACE="${L2TP_INTERFACE_PREFIX}${i}"

  echo "建立L2TP连接 $L2TP_INTERFACE..."
  echo "c l2tp_connection" > /var/run/xl2tpd/l2tp-control
  sleep 5

  # 检查L2TP接口是否已启动
  if ip link show | grep -q "$L2TP_INTERFACE"; then
    echo "L2TP连接 $L2TP_INTERFACE 已成功建立。"
  else
    echo "L2TP连接 $L2TP_INTERFACE 失败，请检查配置。"
    exit 1
  fi

  # 启动SOCKS5服务
  echo "启动SOCKS5代理: 监听端口 $PORT -> 使用接口 $L2TP_INTERFACE"
  socat TCP4-LISTEN:$PORT,fork SOCKS4A:127.0.0.1:$PORT &
done

# 提示完成
echo "所有SOCKS5代理已完成配置，以下端口已启用: $SOCKS5_PORTS"
