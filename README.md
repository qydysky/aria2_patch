### aria2 patch
#### 介绍
本项目配合[qydysky/aria2](https://github.com/qydysky/aria2)使用，fork并clone项目到本地后，将下述patch置于项目`patch/`中，commit并push到github后，将自动触发action构建，产物将放置于release的Draft中（注意：若Draft在构建前存在，将无法正常放置）。

#### patch列表
- [create_input_file_if_not_exist.patch](create_input_file_if_not_exist.patch)  
由于aria2启动时可以通过`-i, --input-file=<FILE>`读取`--save-session=<FILE>`保存的文件，但当首次启动，文件不存在时，会发生`Exception: [download_helper.cc:562] errorCode=1 Failed to open the file`。故有此patch，当文件不存在时，创建一个。

- [tracker_fail_anytime.patch](tracker_fail_anytime.patch)  
由[Aria2 改造Tracker逻辑 ](https://aheadsnail.github.io/2018/12/06/Aria2-%E6%94%B9%E9%80%A0Tracker%E9%80%BB%E8%BE%91/)一文，得出当aria2连接tracker后，将会不再询问其他tracker，这个patch不再调用成功时执行的函数，总是执行失败时的函数，从而一直轮询。但需注意设置轮询间隔，避免tracker返回过于频繁的拒绝响应。

- [tracker_retry_wait.patch](tracker_retry_wait.patch)  
tracker失败时会重试1次，此patch将重试间隔设为`--retry-wait=<SEC>`。

