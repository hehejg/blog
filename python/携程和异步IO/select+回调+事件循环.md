# epoll并不代表一定比select好
## 在并发高的情况下，连接活跃度不高，epoll比select好
## 并发性不高，同时连接很活跃，select比epool好
### 非阻塞IO和阻塞IO对比

#### 通过非阻塞IO实现http请求
