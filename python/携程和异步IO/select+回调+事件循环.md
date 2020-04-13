# epoll并不代表一定比select好
## 在并发高的情况下，连接活跃度不高，epoll比select好
## 并发性不高，同时连接很活跃，select比epool好
### 非阻塞IO和阻塞IO对比

#### 通过非阻塞IO实现http请求
```python
import socket
from urllib.parse import urlparse


# 使用非阻塞IO请求http
def get_url(url):
    # 通过socket请求html
    url = urlparse(url)
    host = url.netloc
    path = url.path
    if path == "":
        path = "/"

    # 建立socket连接
    client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client.setblocking(False)  # 设为False使用非阻塞IO请求
    try:
        client.connect((host, 80))
    except BlockingIOError:
        pass
    while True:
        try:
            client.send(f"GET {path} HTTP/1.1\r\nHost:{host}\r\nConnection:close\r\n\r\n".encode('utf8'))
            break
        except OSError:
            pass

    data = b""
    while True:
        try:
            d = client.recv(1024)
        except BlockingIOError:
            continue
        if d:
            data += d
        else:
            break
    data = data.decode('utf8')
    html_data = data.split("\r\n\r\n")[1]
    print(html_data)
    client.close()


if __name__ == "__main__":
    get_url('http://www.baidu.com')

```
