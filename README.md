# AleOsh.
# This is a Python program that sends reboot packets within a local area network

from tqdm import tqdm
import socket
import struct
# 多播组IP地址，例中选择224.0.0.1，子网掩码为255.0.0.0
MCAST_GRP = '224.0.0.1'
MCAST_MASK = '255.0.0.0'
# 创建UDP套接字并绑定到端口号，例如8888
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM, socket.IPPROTO_UDP)
sock.bind(('0.0.0.0', 8888))
# 设置套接字选项，允许广播和多播
sock.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_MULTICAST_TTL, 2)
# 构造多播数据包，例中指令为重启计算机
message = b'\x01\x01' # 指令为重启计算机，具体含义可以自定义
# 获取局域网内的计算机IP列表
ip_list = ['192.168.10.{}'.format(i) for i in range(1, 256)]  # 修改了网段为192.168.10
# 发送多播数据包并显示进度条和IP信息
for ip in tqdm(ip_list):
    try:
        sock.sendto(message, (ip, 8888))
        print(f"IP: {ip}, Status: Sent")  # 添加了IP和状态信息
    except (ConnectionRefusedError, socket.timeout):
        print(f"IP: {ip}, Status: Error")  # 添加了IP和状态信息
