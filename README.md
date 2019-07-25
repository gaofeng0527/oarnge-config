1. 作为配置中心的文件，文件名最好不要用关键字，比如config
2. 关键信息要加密，使用jdk 8 中的 jce_policy-8.zip 下的加密算法
3. 启动应用使用crul进行加密：curl -X POST http://localhost:7000/encrypt -d root
