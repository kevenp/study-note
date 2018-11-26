# 安装
配置文件：
    clientPort=2481
    dataDir=/data
    dataLogDir=/datalog
    tickTime=2000
    initLimit=5
    syncLimit=2
    maxClientCnxns=60
    server.0=localhost:2888:3888
    server.1=localhost:2988:3988
    server.2=localhost:2898:3898

    docker run  --name some-zookeeper2 --restart always -d -p 2381:2181 -p 2898:2888 -p 3898:3888 -v $(pwd)/zoo2.cfg:/conf/zoo.cfg  zookeeper 

## docker-compose安装
https://segmentfault.com/a/1190000006907443