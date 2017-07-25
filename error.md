
#### 查看 ubuntu 内存 和 CPU

	free -m 



#### error

	(error) MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk.

	原因：数据持续写入，读取速度远低于写入速度，持续1H以上(中途开了一个较长时间的会，一直写入数据，没管)，内存占用量为80%。

	解决：
	1、启动：./redis-cli 
	2、输入：config set stop-writes-on-bgsave-error no
