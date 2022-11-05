Linux后台部署工程
root@localhost app]# nohup java -jar springboot_ssm-0.0.1-SNAPSHOT.jar > server.log 2>&1 &

后台部署工程之后想关掉工程得知道进程的ipd
	查询
		root@localhost app]# ps -ef|grep "java -jar"
	结果
		root      73726  99771 21 17:32 pts/0    00:00:14 java -jar springboot_ssm-0.0.1-SNAPSHOT.jar         ←此进程 pid 73726
		root      75231  99771  0 17:33 pts/0    00:00:00 grep --color=auto java -jar
	关掉进程
		kill -9 73726
	