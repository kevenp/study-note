# netty 粘包和拆包和半包
## 拆包
### 分隔符和定长解码器
    DelimeterBasedFrameDecoder  分隔符
    FixedLengthFrameDecoder     长度
## 半包
    LengthFieldPrepender
在Bytebuf之前增加了n个字节的消息长度字段
    
    LengthFieldBasedFrameDecoder
跳过目标Bytebuf之前的n个字节（由**LengthFieldPrepender**添加）

### 使用示例
    ch.pipeline()
                                    .addLast("frameDecoder", new LengthFieldBasedFrameDecoder(65535,0,2,0,2))
                                    .addLast("msgDepack", new MsgpackDecoder())
                                    .addLast("frameEncoder", new LengthFieldPrepender(2))
                                    .addLast("msgEncoder", new MsgpackEncoder())
                                    .addLast(new EchoClientHandler(sendNum));
    