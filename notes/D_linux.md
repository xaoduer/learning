scp dump-dump-2018-05-24-08-02-09.0001.phd  10.74.148.3:/home/duyl
 
 no such file found err
 vi install.sh
 :set ff
 :set ff=unix
 
 nohup .sh命令  &
 
 性能分析：
 CPU
 vmstat 1   每隔一秒显示一行
 
 CPU空闲可能原因:
 应用被同步原语阻塞，直至锁释放才能继续执行
 应用在等待某些东西，例如数据库调用锁返回的响应
 应用的确是无所事事
 
 磁盘：
 iostat -xm 5
 
 网络：
 netstat  
 nicstat 5
 
 内存占用：
 top  ps  windows:perfmon/VMMap
 
 RSS:Resident Set size 进程驻留集大小。PSS是对RSS的改进，去掉了和其它程序共享的数据。 windows:工作集working set
 
 
 nslookup commsrvkafka
 
 shell
 变量不能传递到子shell，传递请使用export
 
 windows:
 
 netstat -ano | findstr 8083
tasklist | findstr 9836
 taskkill /f /t  /PID 9836
 
 netstat -ano | grep 10081|wc
 
 小技巧:
 CTRL+R
 cd -
 使用 pgrep 和 pkill 来找到或是kill 某个名字的进程。 (-f 选项很有用)
 使用 nohup 或  disown 如果你要让某个进程运行在后台。
 使用netstat -lntp来看看有侦听在网络某端口的进程。当然，也可以使用 lsof
 在bash的脚本中，你可以使用 set -x 来debug输出。使用 set -e 来当有错误发生的时候abort执行。考虑使用 set -o pipefail 来限制错误。还可以使用trap来截获信号（如截获ctrl+c）
 通过 <(some command) 可以把某命令当成一个文件。示例：比较一个本地文件和远程文件 /etc/hosts： diff /etc/hosts <(ssh somehost cat /etc/hosts)
 mtr命令比traceroute要好
 
 du -s * | sort -n | tail
列出当前目录里最大的10个文件。
ps aux | sort -nk +4 | tail
列出头十个最耗内存的进程
netstat –tlnp
列出本机进程监听的端口号。（陈皓注：netstat -anop 可以显示侦听在这个端口号的进程）
tail -f /path/to/file.log | sed '/^Finished: SUCCESS$/ q'
curl ifconfig.me
当你的机器在内网的时候，可以通过这个命令查看外网的IP
lsof –i
实时查看本机网络服务的活动状态


wget -d --header="User-Agent: Mozilla/5.0 (Windows NT 6.0) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.97 Safari/537.11" --header="Referer: http://xmodulo.com/" --header="Accept-Encoding: compress, gzip" http://www.google.com/

wget --header="content-type:application/json"  www.ba.com

curl -H 'X-DexMesh-username:duyl-test' -H 'X-DexMesh-passwd:duyl-test-passwd' http://172.167.20.15:8082/api/zenaptest/v1/client/header/check
curl -H 'X-DexMesh-username:duyl-test' -H 'X-DexMesh-passwd:duyl-test-passwd' https://10.75.8.83:28003/api/zenaptest/v1/client/header/check
curl -X GET --header 'Accept:application/json,X-DexMesh-username:duyl-test,X-DexMesh-passwd:duyl-test-passwd' https://10.75.8.83:28003/api/zenaptest/v1/client/header/check

gzip -d 
gunzip

打包：

tar -cvf  log.tar log2012.log 仅打包，不压缩
tar -zcvf log.tar.gz log2012.log 打包后，以gzip压缩
tar -jcvf log.tar.bz2 log2012.log 打包后，以bzip2压缩

查阅 ：
tar -ztvf log.tar.gz

解压：
tar -zxvf log.tar.gz

apk add --update iproute2 curl iputils drill

/root/zartcli/zartcli -o query -i zenap -m image -n javaaf -v v1.19.20.00

可以使用一下命令查使用内存最多的5个进程

ps -aux | sort -k4nr | head 5

或者

top （然后按下M，注意大写）

ps aux | head -1;ps aux |grep -v PID |sort -rn -k +4 | head -20

 

可以使用一下命令查使用CPU最多的5个进程

ps -aux | sort -k3nr | head 5

或者

top （然后按下P，注意大写）

linux压缩和解压缩命令大全
        .tar
　　解包：tar zxvf FileName.tar
　　打包：tar czvf FileName.tar DirName
　　---------------------------------------------
　　.gz
　　解压1：gunzip FileName.gz
　　解压2：gzip -d FileName.gz
　　压缩：gzip FileName
　　.tar.gz 和 .tgz
　　解压：tar zxvf FileName.tar.gz
　　压缩：tar zcvf FileName.tar.gz DirName
　　---------------------------------------------
　　.bz2
　　解压1：bzip2 -d FileName.bz2
　　解压2：bunzip2 FileName.bz2
　　压缩： bzip2 -z FileName
　　.tar.bz2
　　解压：tar jxvf FileName.tar.bz2
　　压缩：tar jcvf FileName.tar.bz2 DirName
　　---------------------------------------------
　　.bz
　　解压1：bzip2 -d FileName.bz
　　解压2：bunzip2 FileName.bz
　　压缩：未知
　　.tar.bz
　　解压：tar jxvf FileName.tar.bz
　　压缩：未知
　　---------------------------------------------
　　.Z
　　解压：uncompress FileName.Z
　　压缩：compress FileName
　　.tar.Z
　　解压：tar Zxvf FileName.tar.Z
　　压缩：tar Zcvf FileName.tar.Z DirName
　　---------------------------------------------
　　.zip
　　解压：unzip FileName.zip
　　压缩：zip FileName.zip DirName
　　---------------------------------------------
　　.rar
　　解压：rar a FileName.rar
　　压缩：r ar e FileName.rar
　　
　　rar请到：http://www.rarsoft.com/download.htm 下载！
　　解压后请将rar_static拷贝到/usr/bin目录（其他由$PATH环境变量指定的目录也可以）：
　　[root@www2 tmp]# cp rar_static /usr/bin/rar
　　---------------------------------------------
　　.lha
　　解压：lha -e FileName.lha
　　压缩：lha -a FileName.lha FileName
　　
　　lha请到：http://www.infor.kanazawa-it.ac.jp/~ishii/lhaunix/下载！
　　>解压后请将lha拷贝到/usr/bin目录（其他由$PATH环境变量指定的目录也可以）：
　　[root@www2 tmp]# cp lha /usr/bin/
　　---------------------------------------------
　　.rpm
　　解包：rpm2cpio FileName.rpm | cpio -div
　　---------------------------------------------
　　.deb
　　解包：ar p FileName.deb data.tar.gz | tar zxf -
