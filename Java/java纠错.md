# java纠错

## realLen = InputStream.read(byte[len] buf)

这里的read方法每次读入buf的长度不一定是buf的容量，所以
在获取buf内容时要使用buf的长度 realLen:
OutputStream.write(buf, realLen)