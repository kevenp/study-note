# netty 学习
## 快速入门
### 服务端
    EventLoopGroup bossGroup = new NioEventLoopGroup(); //线程组，管理
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try {


            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
                    .channel(NioServerSocketChannel.class)
                    .option(ChannelOption.SO_BACKLOG, 1024)
                    .childHandler(new ChildChannelHandler());

            ChannelFuture f = b.bind(port).sync();

            f.channel().closeFuture().sync();
        }finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
### 客户端
    EventLoopGroup group = new NioEventLoopGroup();

        Bootstrap b = new Bootstrap();
        b.group(group).channel(NioSocketChannel.class)
                .option(ChannelOption.TCP_NODELAY, true)
                .handler(new ChannelInitializer<io.netty.channel.socket.SocketChannel>() {
                    @Override
                    protected void initChannel(io.netty.channel.socket.SocketChannel ch) throws Exception {
                        ch.pipeline()
                                .addLast(new LineBasedFrameDecoder(1024))
                                .addLast(new StringDecoder())
                                .addLast(new TimeClientHandler());
                    }
                });

        ChannelFuture f = b.connect(host, port).sync();
        f.channel().closeFuture().sync();
        group.shutdownGracefully();
### 处理请求
创建handler类，继承`SimpleChannelInboundHandler`。例如
    class TimeClientHandler extends SimpleChannelInboundHandler
## 粘包和拆包
服务端和客户端的沟通基于流，多个消息和在一起（**粘包**），如何将消息分开（**拆包**）