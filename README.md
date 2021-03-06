### 虚拟机网络分配原理

	Windows  ->  网卡
		VMWare	  ->  WMnet8
			Linux
				命令 [-选项] 参数1 参数2 ...
				网络配置：/etc/sysconfig/network-scripts/
							ifcfg-ens33:
								IPADDR
								GATEWAY
								NETMASK
								DNS1
### 命令
	cd 进入目录，不能进入文件(绝对/相对)
	
	ll 查看当前文件夹下的所有非隐藏的内容和属性(绝对/相对)，相当于 ls -l
	
	ls 目标目录
	
	ls -al 查看目录下所有内容(包括隐藏文件)
	
	mkdir [-p] 创建目录 单级\多级
	
	rm -[rf] 删除目录或文件[单级/多级]
	
	mv {old} {new} :更改文件名
	
	touch 创建文件的路径(单级/多级)
		
	cat 要查看的文件的内容
	
	pwd 查看所在路径
	
	scp [-r] 文件原路径 服务器(root@ip):拷贝路径
	
	tar -zcvf 压缩文件名.tar.gz 目标目录
		可先切换到当前目录下，压缩文件名和被压缩文件名都可加入路径。
	
	tar -zxvf 压缩文件名.tar.gz
		解压缩后的文件只能放在当前的目录。
	
	reboot 重启
	
	shutdown
		-t seconds:设定在几秒钟之内进行关机程序
		-k:并不会真的关机，只是将警告传递给所有使用者
		-r:关机后重启
		-h:关机后停机
		-n:不采用正常程序关机，用强迫的方式杀掉所有执行程序后自行关机
	
	poweroff 命令关闭 Linux 系统，关闭记录会被写入到 /var/log/wtmp 日志文件中
		
	vi /etc/sysconfig/network-scripts/ifcfg-ens33 更改网络配置
	
	vi /etc/hostname 更改主机名
	
	vi /etc/hosts 将ip与主机名进行绑定
	
	ssh hadoop101 免密登陆
	
	xsync 分发(远程同步工具，主要用于备份和镜像)
	
	sudo find / -name mysql	在根目录下查找包含mysql的所有文件
		-name 按命名
	
	rpm -qa | grep
		-q 表示查询，是 query 的首字母。
		-a 表示系统中所有
	
	rpm -e '包名'	卸载
		-e 选项表示卸载，也就是 erase 的首字母。
	
	rpm -ivh '包名'	安装	
		-i：安装（install）;
		-v：显示更详细的信息（verbose）；
		-h：打印 #，显示安装进度（hash）;
	
	用户及权限分配
		1、用户分类
			u: 所有者
			g: 所属组
			o: 其他用户
			a: 所有用户
		2、用户管理
		id user: 查看用户
		useradd user: 添加用户
		passwd user: 设置密码
		userdel -r uesr: 删除用户
		3、用户与组
		gpasswd -a testuser root: 加入组
			解释：把用户testuser加入到root组，加入组后testuser获取到user组和root组所有权限
		gpasswd -d testuser root: 移出组
			解释：把用户testuser移出root组
		4、权限分类
		r: 读 4
		w: 写 2
		x: 执行 1
		ls -l file: 查看文件权限（或ll file）
		eg:
			drw-r-xr-- 3 root root 4096 Jun 21 2021 file
				解释：root用户对file文件具有rw权限
					root组其他用户对file文件具有读(r)、执行(x)权限
					其他用户对file文件具有只读(r)权限
					第一个root为创建file文件的所有者
					第二个root为创建file文件的所属组
		5、chmod权限分配
		chmod u+x file 字母权限分配
			eg:
				chmod u+x,g+w,o+w test.txt
				chmod u-x,g-w,o-w test.txt
		chmod 755 file: 数字权限分配
			755表示: -rwxr-xr-x
				解释：7: rwx
						5: r-x
						5:r-x

### 文本编辑器

	vi linux自带文本编辑器
		vi 文件的路径
			vi test.txt 打开test.txt文件
			i 进入编辑模式
			esc 退出编辑模式
			shift+; 进入命令模式
			q! 仅退出
			wq 保存并退出

### 网络工程
	上网需要什么
		ip地址：在网络上唯一标识一台计算机节点的地址值
		网关：网络出口
		子网掩码：判断ip地址类型的标准  
		DNS服务器地址：对域名进行解析的服务器

### Linux交互
	1、关闭防火墙。(华为、h3c、思科)
		Linux关闭防火墙：
			1、systemctl status firewalld 查看防火墙
			
			2、systemctl restart firewalld 重启防火墙
			
			3、systemctl stop firewalld 停止防火墙
			
			4、systemctl disable firewalld 关闭防火墙自启


### 生成密钥及发送公钥
	cd .ssh/ 进入.ssh文件夹
	
	ssh-keygen -t rsa 生成密钥对
	
	ssh-copy-id {ip} 将公钥发送给{ip}主机
	
	ssh {ip}主机名 免密登陆


### 分布式文件系统
	对于庞大的文件，进行拆分，并分散储存在多个服务器节点中，从多个服务器中并非读取对应的数据片段，提高读写效率
	例如：
		apache hadoop
			HDFS	Hadoop Distribute File System
						分布式存储
						数据块的方式来存储数据的
						副本机制保证数据的可靠性
					1、rm -rf data logs 删除每台主机/opt/driver/hadoop-3.1.3下的data和logs
					2、hdfs namenode -format 格式化
					3、sbin/stop-dfs.sh
					4、jps
					5、sbin/start-dfs.sh
					6、jps
					7、浏览器打开 hadoop101(192.168.116.101):9870
					
					1、[hwhadoop@hadoop101 hadoop-3.1.3]# sbin/stop-dfs.sh 关闭停止hadoop服务
					3、shutdown -h now	立刻关闭虚拟机
					4、clusterID集群ID，namenode、datanode、secondaryNamenode 应具备相同的clusterID
					5、关闭所有服务、重新格式化、再开启服务
				
						1、关闭服务，同上
						2、删除所有主机hadoop-3.1.3目录下的data和logs目录 rm -rf data logs
						3、格式化：bdfs namenode -format
						4、在101上启动服务：sbin/start-dfs.sh
					注意所有操要在hadoop-3.1.3目录下进行
					仅在hadoop101上进行操作集群：
					运行脚本：hadoopall.sh
					查看所有集群的的链接情况：jpsall
			
			MapReduce
				resouceManager
				nodemansger
					sbin/start-yarn.sh	启动yarn
			Yarn
	
		HDFS
		优点：
			1、高容错率
			2、适合处理大型数据、可以处理GB、TB、甚至PB级别的数据
			3、可以构建在廉价的机器上
		缺点：
			1、不适合低延时的数据访问
			2、无法高效的对大量的小文件进行储存
			3、不支持并发写入
	
	  	hadoop fs -put [本地地址] [远程地址] 上传文件
	  
	  	hadoop fs -moveFromLocal [本地地址] [远程地址] 从本地移动文件到远程
	  	
	  	hadoop fs -copyFromLocal [本地地址] [远程地址] 从本地拷贝文件到远程
	  	
	  	hadoop fs -copyToLocal [远程地址] [本地地址] 从远程拷贝文件到本地
	  	
	  	hadoop fs -get [远程地址] [本地地址] 从远程拉取文件到本地
	  	
	  	hadoop fs -du -s -h [远程地址] 查看远程地址文件总信息
	  	
	  	hadoop fs -mv [源文件路径] [目标路径]   （相对路径）
	 		
	 		hadoop fs -rm [文件的路径(绝对路径)] 删除文件
	 		
	 		hadoop fs -rm -r [目录(绝对路径)] 删除目录
	 		

### (Hive) Hsql  mysql

	show databases;	显示所有库
	
	use '库名';	进入库
	
	show tables;	显示所有表
	
	create table temp(id int); 创建temp表，temp表创建id列，属性为int型
	
	insert into temp(id) values(1001);	在temp表里的id列插入1001
	
	select * from temp;	查询temp表中所有内容

### mysql>
	mysql -u'用户名' -p'密码'	登陆mysql
		
	set password=password("");	更改密码
	
	update mysql.user set host='%' where user='root';  配置远程登录mysql
	
	flush privileges;	刷新MySQL的系统权限相关表­
	
	quit;

