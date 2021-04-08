### aria2 patch
#### 介绍
本项目配合[qydysky/aria2](https://github.com/qydysky/aria2)使用，fork并clone项目到本地后，将下述patch置于项目`patch/`中，commit并push到github后，将自动触发action构建，产物将放置于release的Draft中（注意：若Draft在构建前存在，将无法正常放置）。

#### patch列表
- [create_input_file_if_not_exist.patch](create_input_file_if_not_exist.patch)  
由于aria2启动时可以通过`-i, --input-file=<FILE>`读取`--save-session=<FILE>`保存的文件，但当首次启动，文件不存在时，会发生`Exception: [download_helper.cc:562] errorCode=1 Failed to open the file`。故有此patch，当文件不存在时，创建一个。

- [trackers_boost.patch](trackers_boost.patch)  
由[Aria2 改造Tracker逻辑 ](https://aheadsnail.github.io/2018/12/06/Aria2-%E6%94%B9%E9%80%A0Tracker%E9%80%BB%E8%BE%91/)一文，得出当aria2连接tracker后，将会不再询问其他tracker，这个patch不再调用成功时执行的函数，总是执行失败时的函数，从而一直轮询。当轮询到开始时，会等待`--bt-tracker-interval=<SEC>`那么多秒，避免短时间内重复暂停启动导致的tracker拒绝。该patch将会将`bt-tracker-interval`默认设置为60s，`bt-tracker-connect-timeout`默认设置为7s，`bt-tracker-timeout`默认设置为7s。

- [tracker_retry_wait.patch](tracker_retry_wait.patch)  
tracker失败时会重试1次，此patch将重试间隔设为`--retry-wait=<SEC>`。

- [socket_rec_buffer_size.patch](socket_rec_buffer_size.patch)  
设置`socket_rec_buffer_size=512k`。关于这个patch的详情见[aria2缓冲区](https://blog.ntsdtt.bid/2021/41d62b99d1f0.html)
