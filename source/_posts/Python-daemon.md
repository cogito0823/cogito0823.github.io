---
title: Python 的守护进程
date: 
mathjax: false
tags: [python]
categories: python
---

在工作中会出现这样的场景：启动一个服务，需要让这个服务在后台运行，又不受其运行前的环境的影响（这些环境包括未关闭的文件描述符、控制终端、会话和进程组、工作目录以及文件创建掩码等）。

这时可以使用守护进程来控制程序的运行。

![pexels-matthias-zomer-98014](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/pexels-matthias-zomer-98014.jpg)

<!--more-->

使程序后台运行的一般的做法有用 `&` 把这个服务放在后台运行，加上 `nohup`，使得服务在终端关闭的情况下也不会停止。但这种做法在我实际使用中出现了意想不到的错误——我在使用 `subprocess.Popen` 来使用多进程过程中，在执行的语句里使用了 `time.sleep()` 进行睡眠，这时 `nohup` 控制的程序进程意外停止进程却未消失，而在不使用 `nohup` 将后台挂起的方法时，程序却能正常运行，没有停止。

于是考虑使用 python 编写守护进程来替代 `nohup` 方式。

**daemon.py**

```python
# !/usr/bin/env python
# coding: utf-8

# python模拟linux的守护进程
import sys, os, time, atexit
from datetime import datetime
from signal import SIGTERM

from AutoTestMain import AutoTest


class Daemon:
    def __init__(self, pidfile, stdout='/dev/null', stderr='/dev/null'):
        # 需要获取调试信息，改为stdout='/dev/stdout', stderr='/dev/stderr'，以root身份运行。
        self.stdout = stdout
        self.stderr = stderr
        self.pidfile = pidfile

    def _daemon(self):
        try:
            pid = os.fork()  # 第一次fork，生成子进程，脱离父进程
            if pid > 0:
                sys.exit(0)  # 退出主进程
        except OSError as e:
            sys.stderr.write('fork #1 failed: %d (%s)\n' % (e.errno, e.strerror))
            sys.exit(1)

        os.chdir("/")  # 修改工作目录
        os.setsid()  # 设置新的会话连接
        os.umask(0)  # 重新设置文件创建权限

        try:
            pid = os.fork()  # 第二次fork，禁止进程打开终端
            if pid > 0:
                sys.exit(0)
        except OSError as e:
            sys.stderr.write('fork #2 failed: %d (%s)\n' % (e.errno, e.strerror))
            sys.exit(1)

        # 重定向文件描述符
        sys.stdout.flush()
        sys.stderr.flush()
        si = open('/dev/null', 'r')
        so = open(self.stdout, 'ab+')
        se = open(self.stderr, 'ab+', 0)
        os.dup2(si.fileno(), sys.stdin.fileno())
        os.dup2(so.fileno(), sys.stdout.fileno())
        os.dup2(se.fileno(), sys.stderr.fileno())

        # 注册退出函数，根据文件pid判断是否存在进程
        atexit.register(self.delpid)
        pid = str(os.getpgid(os.getpid()))	# 获取进程组 id
        open(self.pidfile, 'w+').write('%s\n' % pid)

    def delpid(self):
        os.remove(self.pidfile)

    def start(self):
        # 检查pid文件是否存在以探测是否存在进程
        try:
            pf = open(self.pidfile, 'r')
            pid = int(pf.read().strip())
            pf.close()
        except IOError:
            pid = None

        if pid:
            message = 'pidfile %s already exist. Daemon already running!\n'
            sys.stderr.write(message % self.pidfile)
            sys.exit(1)

        # 启动监控
        self._daemon()
        self._run()

    def stop(self):
        # 从pid文件中获取pid
        try:
            pf = open(self.pidfile, 'r')
            pid = int(pf.read().strip())
            pf.close()
        except IOError:
            pid = None

        if not pid:  # 重启不报错
            message = 'pidfile %s does not exist. Daemon not running!\n'
            sys.stderr.write(message % self.pidfile)
            return

        # 杀进程
        try:
            while 1:
                os.killpg(pid, SIGTERM)
                time.sleep(0.1)
                # os.system('hadoop-daemon.sh stop datanode')
                # os.system('hadoop-daemon.sh stop tasktracker')
                # os.remove(self.pidfile)
        except OSError as err:
            err = str(err)
            if err.find('No such process') > 0:
                if os.path.exists(self.pidfile):
                    os.remove(self.pidfile)
            else:
                print(str(err))
                sys.exit(1)

    def restart(self):
        self.stop()
        self.start()

    def _run(self):
        """ run your fun"""
        while True:
            # fp=open('/tmp/result','a+')
            # fp.write('Hello World\n')
            sys.stdout.write('%s:hello world\n' % (time.ctime(),))
            sys.stdout.flush()
            time.sleep(2)


# 定义子类，重写run()方法实现自己的功能。
class MyDaemon(Daemon):
    def _run(self):
        autoTest = AutoTest()
        logger = autoTest.getLogger()
        logger.warn('开启子线程，start time=' + datetime.now().strftime('%Y-%m-%d %H:%M:%S %f'))
        autoTest.start()


if __name__ == '__main__':

    # 路径
    current_path = os.path.abspath('.')
    pid_path = os.path.join(current_path, 'logs/watch_process.pid')
    stdout_path = os.path.join(current_path, 'logs/watch_stdout.log')
    stderr_path = os.path.join(current_path, 'logs/watch_error.log')

    daemon = MyDaemon(pidfile=pid_path, stdout=stdout_path, stderr=stderr_path)
    if len(sys.argv) == 2:
        if 'start' == sys.argv[1]:
            daemon.start()
        elif 'stop' == sys.argv[1]:
            daemon.stop()
        elif 'restart' == sys.argv[1]:
            daemon.restart()
        else:
            print('unknown command')
            sys.exit(2)
        sys.exit(0)
    else:
        print('usage: %s start|stop|restart' % sys.argv[0])
        sys.exit(2)
```

**run.py**

```
import sys, time
  
def run():
        while True:
            #fp=open('/tmp/run.log','a+')  
            #fp.write('Hello World\n')  
            # sys.stdout.write('%s:lll\n' % (time.ctime(),))
            # sys.stdout.flush()
            # time.sleep(1)
            print('%s:your message\n' % (time.ctime(),))
            time.sleep(1)
```

路径拼接：

```
>>> path1=os.path.abspath('.')
'/root/learningPython/utils'
>>> os.path.join(path1, 'ss')
'/root/learningPython/utils/ss'
```

### daemon.py 的使用

- 启动：`python daemon.py start`
- 停止：`python daemon.py stop`
- 重启：`python daemon.py restart`

logs 目录

- `watch_stdout.log`：日志文件
- `watch_process.pid`：进程号文件，`daemon.py` 程序根据这个文件是否存在来判断服务有没有在运行。
  - 当使用 `python daemon.py start` 启动服务时，daemon.py 会新建一个 watch_process.pid 文件
  - 当使用 `python daemon.py stop` 停止服务时，daemon.py 会删除 watch_process.pid 文件

强杀进程：如果 python 服务进程存在，但 watch_pid 文件不存在，那么使用 `python daemon.py stop`将无法停止 python 服务，这时可以查出 python 服务进程的进程组 id ，然后使用 kill -9 -id 来将一组进程停止。

查看进程关系：

```
$ ps j -A | grep python
PPID    PID  PGID   SID TTY      TPGID STAT   UID   TIME COMMAND
    1 10567 10566 10566 ?           -1 Sl       0   0:00 python daemon.py start
10567 10571 10566 10566 ?           -1 Sl       0   0:03 python daemon.py start
10571 10851 10566 10566 ?           -1 S        0   0:09 python daemon.py start
10571 10852 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10853 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10854 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10855 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10856 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10857 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10858 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10859 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10860 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10861 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10862 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10863 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10864 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10865 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10866 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10867 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10868 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10869 10566 10566 ?           -1 S        0   0:00 python daemon.py start
10571 10870 10566 10566 ?           -1 S        0   0:00 python daemon.py start
13451 25752 25751 13451 pts/4    25751 S+    1000   0:00 grep --color=auto python
```

如上的进程组，可以用 `kill -9 -10566` 将一组进程杀死。

进程树：

```shell
$ cat ./logs/watch_process.pid
10567
$ pstree -p | grep python
           |-python(10567)-+-python(10571)-+-python(10851)---sh(17841)
           |               |               |-python(10852)
           |               |               |-python(10853)
           |               |               |-python(10854)
           |               |               |-python(10855)
           |               |               |-python(10856)
           |               |               |-python(10857)
           |               |               |-python(10858)
           |               |               |-python(10859)
           |               |               |-python(10860)
           |               |               |-python(10861)
           |               |               |-python(10862)
           |               |               |-python(10863)
           |               |               |-python(10864)
           |               |               |-python(10865)
           |               |               |-python(10866)
           |               |               |-python(10867)
           |               |               |-python(10868)
           |               |               |-python(10869)
           |               |               |-python(10870)
           |               |               |-{python}(10871)
           |               |               |-{python}(10872)
           |               |               `-{python}(10873)
           |               |-{python}(10579)
           |               |-{python}(10675)
           |               |-{python}(13915)
           |               |-{python}(14005)
           |               |-{python}(17164)
           |               |-{python}(17254)
           |               |-{python}(20442)
           |               |-{python}(20619)
           |               |-{python}(23928)
           |               `-{python}(24278)
```

```shell
$ cat ./logs/watch_process.pid
10567
$ ps -e --forest | grep python
10567 ?        00:00:00 python
10571 ?        00:00:02  \_ python
10851 ?        00:00:09      \_ python
10852 ?        00:00:00      \_ python
10853 ?        00:00:00      \_ python
10854 ?        00:00:00      \_ python
10855 ?        00:00:00      \_ python
10856 ?        00:00:00      \_ python
10857 ?        00:00:00      \_ python
10858 ?        00:00:00      \_ python
10859 ?        00:00:00      \_ python
10860 ?        00:00:00      \_ python
10861 ?        00:00:00      \_ python
10862 ?        00:00:00      \_ python
10863 ?        00:00:00      \_ python
10864 ?        00:00:00      \_ python
10865 ?        00:00:00      \_ python
10866 ?        00:00:00      \_ python
10867 ?        00:00:00      \_ python
10868 ?        00:00:00      \_ python
10869 ?        00:00:00      \_ python
10870 ?        00:00:00      \_ python
```

![image-20201209145303683](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201209145303683.png)

## 参考链接

具体的原理可以参考：

- [Python 守护进程 - OSCHINA - @lang13002](https://my.oschina.net/u/3021599/blog/4318309)
- [Killing a process and all of its descendants - morningcoffee.io - @igor_sarcevic](http://morningcoffee.io/killing-a-process-and-all-of-its-descendants.html)
- [[译] 如何杀死一个进程和它的所有子进程 - 掘金 - @江不知](https://juejin.cn/post/6844903927989665806?utm_campaign=devtool.com&utm_medium=devtool.com&utm_medium=devtool.com&utm_source=devtool.com%3Futm_campaign%3Ddevtool.com&utm_source=devtool.com)
