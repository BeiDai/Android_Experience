# Connector 网络连接

- socket和输入输出流定义

      Socket socket = null;
      DataInputStream din = null;
      DataOutputStream dout = null;

- 网络连接函数

       public MyConnector(String address,int port){
           try{
               socket = new Socket(address,port);
               din = new DataInputStream(socket.getInputStream());
               dout = new DataOutputStream(socket.getOutputStream());
           }catch (Exception e){
               e.printStackTrace();
           }
       }

- 连接断开函数

       public void sayBye(){
           try{
               dout.writeUTF("<#USER_LOGOUT#>");
               din.close();
               dout.close();
               socket.close();
               socket = null;
           }catch (Exception e){
               e.printStackTrace();
           }
       }

## 使用函数方法

- 建立socket

      // 根据IP和端口号建立连接
      mc = new MyConnector(SERVER_ADDRESS,SERVER_PORT);

- 发送消息

      String regInfo = "<#REGISTER#>" + name + "|" + pwd1 + "|" + email ;
      mc.dout.writeUTF(regInfo);

- 接收消息

      String result = mc.din.readUTF();

- 消息处理

      if(result.startsWith("<#REG_SUCCESS#>")){
                              result = result.substring(15);
      // 如果消息头为 <#REG_SUCCESS#>
      // 取result的15位以后的数据   
                              Log.i("AppDebug",result);
                         }
