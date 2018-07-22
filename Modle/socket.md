# Socket 使用经验

### 获取联网权限

     <uses-permission android:name="android.permission.INTERNET" />

### 写SocketUtil自定义类

    // 实现创建socket发送数据功能
    public class SocketUtil {

        private String str;
        private Socket socket;
        private String ip;

        public SocketUtil(String str, String ip){
            this.str = str;
            this.ip = ip;
        }

        public String sendMessage(){
            String result = "123134";
            try{
                socket = new Socket();
                socket.connect(new InetSocketAddress(ip, 8086),5000);
                OutputStream out = socket.getOutputStream();
                out.write(str.getBytes());
                out.flush();
                out.close();
                socket.close();
                Log.i("AppDebug","close");
            }catch (SocketTimeoutException e){
                Log.i("socket",e.toString());
            }catch (IOException e){
                e.printStackTrace();
            }
            return result;
        }
    }

### 主界面里调用

    // 发送数据给ip：192.168.4.1
    private void initView(){
         textView = (TextView) findViewById(R.id.book);
         final EditText editText = (EditText) findViewById(R.id.message);
         button = (Button) this.findViewById(R.id.button);
         Log.i("AppDebug","initView");
         button.setOnClickListener(new View.OnClickListener() {
             @Override
             public void onClick(View view) {
                 Log.i("AppDebug","Thread");
                 new Thread(new Runnable() {
                     @Override
                     public void run() {
                         Log.i("AppDebug","run");
                         result = new SocketUtil(editText.getText().toString(),"192.168.4.1").sendMessage();
                     }
                 }).start();
             }
         });
     }
