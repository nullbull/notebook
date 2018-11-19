# [NIO](https://www.ibm.com/developerworks/cn/education/java/j-nio/j-nio.html)

### Buffer

1. 两个buffer相等,当且仅当剩余内容相同

2. 可以批量传输, 将一个数组作为get的参数,这个数组的大小不能超过缓冲区剩余的内容

3. mark() 将 mark 设为 position reset() 将position设为 mark

4. ```
   char[] zwt = "zwtmylove".toCharArray();
   buffer.put(zwt);
   CharBuffer charBuffer = CharBuffer.wrap(zwt);
   ```

   可以这样初始化一个Buffer

5. 大端字节顺序,最高字节在前,小端字节顺序,最低字节在前, ByteOrder封装了这一特性

6. 直接缓冲区很快

7. 不能获取比缓冲区多的原始类型,比如long 8个字节, 如果缓冲区只有4个字节就会报错

### Channel

1. 不能直接创建FileChannel,只能通过一些流的getChannel获取

2. 只有面向流的Channel才能非堵塞, fileChannel是堵塞的

3. 调用channel的close方法,即使是非堵塞的也会堵塞一下

4. 如果一个线程在一个通道上被阻塞并且同时被中断(由调用该被阻塞线程的 interrupt( )方法的另一个线程中断),那么该通道将被关闭,该被阻塞线程也会产生一个 ClosedByInterruptException 异常

5. 当一个通道被关闭时,休眠在该通道上的所有线程都将被唤醒并接收到一个 AsynchronousCloseException 异常。接着通道就被
   关闭并将不再可用。

6. FileChannel是线程安全的

7. FileChannel的read(position) 和write(position)方法不会修改position

8. 当需要减少一个文件的 size 时,truncate( )方法会砍掉您所指定的新 size 值之外的所有数据。如果当前 size 大于新 size,超出新 size 的所有字节都会被悄悄地丢弃。如果提供的新 size 值大于或等于当前的文件 size 值,该文件不会被修改。这两种情况下,truncate( )都会产生副作用:文件的position 会被设置为所提供的新 size 值。

9. force()强迫缓冲区同步到磁盘

10. fileChannel 对文件加锁, 是调用文件系统对问价加锁,由文件系统管理,如果出现竞争,只能按照先后顺序来

11. 对文件加锁之后一定要调用FileLock.release()方法

12. 内存映射文件是指0拷贝,而且不会占JVM堆内存

13. FileChannel.MapMode.PRIVATE 自己更新的部分自己可见,
    其他对文件的更改在非修复位置也能看到

14. 通道 to 通道 的方式必须有一方是FileChannel

15. SocketChannel会创建对应的Socket

16. 如果以非阻塞模式被调用,当没有传入连接在等待时,ServerSocketChannel.accept( )会立即返回 null。

17. connect( )方法尚未被调用。那么将产生 NoConnectionPendingException 异常。
     连接建立过程正在进行,尚未完成。那么什么都不会发生,finishConnect( )方法会立即返回
    false 值。
     在非阻塞模式下调用 connect( )方法之后,SocketChannel 又被切换回了阻塞模式。那么如果
    有必要的话,调用线程会阻塞直到连接建立完成,finishConnect( )方法接着就会返回 true
    值。
     在初次调用 connect( )或最后一次调用 finishConnect( )之后,连接建立过程已经完成。那么
    SocketChannel 对象的内部状态将被更新到已连接状态,finishConnect( )方法会返回 true
    值,然后 SocketChannel 对象就可以被用来传输数据了。
     连接已经建立。那么什么都不会发生,finishConnect( )方法会返回 true 值。

18. SocketChannel 对应TCP/IP,DatagramChannel对应 UDP/IP

19. Channels里有一些工厂方法,可以对IO和NIO互相封装

20. 与Selector一起使用时,Channel必须处在非堵塞模式下

21. 服务端

    ```java
    package netty.NIO;
    
    import java.io.IOException;
    import java.net.InetSocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.CharBuffer;
    import java.nio.channels.*;
    import java.util.Iterator;
    
    public class Server {
        private ByteBuffer readBuffer;
        private Selector selector;
    
        public static void main(String[] args){
            Server xiaona = new Server();
            xiaona.init();
            xiaona.listen();
        }
    
        private void init(){
            readBuffer = ByteBuffer.allocate(1024);
            ServerSocketChannel servSocketChannel;
    
            try {
                servSocketChannel = ServerSocketChannel.open();
                servSocketChannel.configureBlocking(false);
                //绑定端口
                servSocketChannel.socket().bind(new InetSocketAddress(8383));
    
                selector = Selector.open();
                servSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    
        private void listen() {
            while(true){
                try{
                    selector.select();
                    Iterator ite = selector.selectedKeys().iterator();
    
                    while(ite.hasNext()){
                        SelectionKey key = (SelectionKey) ite.next();
                        ite.remove();//确保不重复处理
                        handleKey(key);
                    }
                }
                catch(Throwable t){
                    t.printStackTrace();
                }
            }
        }
    
        private void handleKey(SelectionKey key) throws IOException, ClosedChannelException {
            SocketChannel channel = null;
    
            try{
                if(key.isAcceptable()){
                    ServerSocketChannel serverChannel = (ServerSocketChannel) key.channel();
                    channel = serverChannel.accept();//接受连接请求
                    channel.configureBlocking(false);
                    channel.register(selector, SelectionKey.OP_READ);
                }
                else if(key.isReadable()){
                    channel = (SocketChannel) key.channel();
                    readBuffer.clear();
                    /*当客户端channel关闭后，会不断收到read事件，但没有消息，即read方法返回-1
                     * 所以这时服务器端也需要关闭channel，避免无限无效的处理*/
                    int count = channel.read(readBuffer);
    
                    if(count > 0){
                        //一定需要调用flip函数，否则读取错误数据
                        readBuffer.flip();
                        /*使用CharBuffer配合取出正确的数据
                        String question = new String(readBuffer.array());  
                        可能会出错，因为前面readBuffer.clear();并未真正清理数据
                        只是重置缓冲区的position, limit, mark，
                        而readBuffer.array()会返回整个缓冲区的内容。
                        decode方法只取readBuffer的position到limit数据。
                        例如，上一次读取到缓冲区的是"where", clear后position为0，limit为 1024，
                        再次读取“bye"到缓冲区后，position为3，limit不变，
                        flip后position为0，limit为3，前三个字符被覆盖了，但"re"还存在缓冲区中，
                        所以 new String(readBuffer.array()) 返回 "byere",
                        而decode(readBuffer)返回"bye"。            
                        */
                        CharBuffer charBuffer = CharsetHelper.decode(readBuffer);
                        String question = charBuffer.toString();
                        String answer = getAnswer(question);
                        channel.write(CharsetHelper.encode(CharBuffer.wrap(answer)));
                    }
                    else{
                        //这里关闭channel，因为客户端已经关闭channel或者异常了
                        channel.close();
                    }
                }
            }
            catch(Throwable t){
                t.printStackTrace();
                if(channel != null){
                    channel.close();
                }
            }
        }
    
        private String getAnswer(String question){
            String answer = null;
    
            switch(question){
                case "who":
                    answer = "我是小娜\n";
                    break;
                case "what":
                    answer = "我是来帮你解闷的\n";
                    break;
                case "where":
                    answer = "我来自外太空\n";
                    break;
                case "hi":
                    answer = "hello\n";
                    break;
                case "bye":
                    answer = "88\n";
                    break;
                default:
                    answer = "请输入 who， 或者what， 或者where";
            }
    
            return answer;
        }
    }
    ```

    客户端

    ```java
    package netty.NIO;
    
    
    import java.io.IOException;
    import java.net.InetSocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.CharBuffer;
    import java.nio.channels.SelectionKey;
    import java.nio.channels.Selector;
    import java.nio.channels.SocketChannel;
    import java.util.Iterator;
    import java.util.Random;
    import java.util.concurrent.ArrayBlockingQueue;
    import java.util.concurrent.BlockingQueue;
    import java.util.concurrent.TimeUnit;
    
    public class Client implements Runnable{
        private BlockingQueue<String> words;
        private Random random;
    
        public static void main(String[] args) {
            //种多个线程发起Socket客户端连接请求
            for(int i=0; i<10; i++){
                Client c = new Client();
                c.init();
                new Thread(c).start();
            }
        }
    
        @Override
        public void run() {
            SocketChannel channel = null;
            Selector selector = null;
            try {
                channel = SocketChannel.open();
                channel.configureBlocking(false);
                //请求连接
                channel.connect(new InetSocketAddress("localhost", 8088));
                selector = Selector.open();
                channel.register(selector, SelectionKey.OP_CONNECT);
                boolean isOver = false;
    
                while(! isOver){
                    selector.select();
                    Iterator ite = selector.selectedKeys().iterator();
                    while(ite.hasNext()){
                        SelectionKey key = (SelectionKey) ite.next();
                        ite.remove();
    
                        if(key.isConnectable()){
                            if(channel.isConnectionPending()){
                                if(channel.finishConnect()){
                                    //只有当连接成功后才能注册OP_READ事件
                                    key.interestOps(SelectionKey.OP_READ);
    
                                    channel.write(CharsetHelper.encode(CharBuffer.wrap(getWord())));
                                    sleep();
                                }
                                else{
                                    key.cancel();
                                }
                            }
                        }
                        else if(key.isReadable()){
                            ByteBuffer byteBuffer = ByteBuffer.allocate(128);
                            channel.read(byteBuffer);
                            byteBuffer.flip();
                            CharBuffer charBuffer = CharsetHelper.decode(byteBuffer);
                            String answer = charBuffer.toString();
                            System.out.println(Thread.currentThread().getId() + "---" + answer);
    
                            String word = getWord();
                            if(word != null){
                                channel.write(CharsetHelper.encode(CharBuffer.wrap(word)));
                            }
                            else{
                                isOver = true;
                            }
                            sleep();
                        }
                    }
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            finally{
                if(channel != null){
                    try {
                        channel.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
    
                if(selector != null){
                    try {
                        selector.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    
        private void init() {
            words = new ArrayBlockingQueue<String>(5);
            try {
                words.put("hi");
                words.put("who");
                words.put("what");
                words.put("where");
                words.put("bye");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
    
            random = new Random();
        }
    
        private String getWord(){
            return words.poll();
        }
    
        private void sleep() {
            try {
                TimeUnit.SECONDS.sleep(random.nextInt(10));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    
        private void sleep(long l) {
            try {
                TimeUnit.SECONDS.sleep(l);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    ```

