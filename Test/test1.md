# 从网络获取图片与网页

- 通过HTTP协议访问URL地址的图片，并显示进度。
- 异步、多线程
- handle通讯
- 网络图片二进制流的读取

---

1、首先在AndroidManifest.xml中添加网络权限

2、其次activity_main.xml界面布局

3、编写MainActivity.java ImageService.java StreamTool.java

---
AndroidMainfest.xml

     <uses-permission android:name="android.permission.INTERNET"></uses-permission>

---
activity_main.xml

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/urlpath" />

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="https://gss3.bdstatic.com/-Po3dSag_xI4khGkpoWK1HF6h
            hy/baike/c0%3Dbaike80%2C5%2C5%2C80%2C26/sign=5a272eb5632762d0
            9433acedc185639f/95eef01f3a292df513353d83bc315c6035a873b6.jpg"
            android:id="@+id/urlpath"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/button"
            android:id="@+id/button"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center_vertical|center_horizontal"
            android:id="@+id/imageView"/>

    </LinearLayout>

---
ImageService.java

    public class ImageService {

        public static byte[] getImage(String path) throws Exception {
            URL url = new URL(path);
            HttpURLConnection conn = (HttpURLConnection)url.openConnection();
            conn.setRequestMethod("GET");
            conn.setConnectTimeout(5 * 1000);
            InputStream inStream = conn.getInputStream();//通过输入流获取图片数据
            return StreamTool.readInputStream(inStream);//得到图片的二进制数据
        }

    }

---
StreamTool.java

    public class StreamTool {

        /**
         * 从输入流中获取数据
         * @param inStream 输入流
         * @return
         * @throws Exception
         ** /
        public static byte[] readInputStream(InputStream inStream) throws Exception{
            ByteArrayOutputStream outStream = new ByteArrayOutputStream();
            byte[] buffer = new byte[1024];
            int len = 0;
            while( (len=inStream.read(buffer)) != -1 ){
                outStream.write(buffer, 0, len);
            }
            inStream.close();
            return outStream.toByteArray();
        }
    }

---
MainActivity.java

    public class MainActivity extends AppCompatActivity {

        private static final String TAG = "MainActivity";
        private EditText pathText;
        private TextView imageView;
        private ImageTask task;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            pathText = (EditText) this.findViewById(R.id.urlpath);
            imageView = (TextView) this.findViewById(R.id.imageView);
            Button button = (Button) this.findViewById(R.id.button);
            button.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    task = new ImageTask();
                    task.execute(pathText.getText().toString());
                }
            });
        }

        class ImageTask extends AsyncTask<String,Integer,byte[]>{

            @Override
            protected void onPreExecute() {
                imageView.setText("图片开始加载...");
            }

            @Override
            protected void onPostExecute(byte[] result) {
                imageView.setText("图片加载完毕");
                try{
                    Thread.sleep(100);
                }catch (InterruptedException e){
                    e.printStackTrace();
                }
                imageView.setText("");
                Bitmap bitmap = BitmapFactory.decodeByteArray(result,0,result.length);
                imageView.setBackground(new BitmapDrawable(getResources(),bitmap));
            }

            @Override
            protected void onProgressUpdate(Integer... values) {
                imageView.setText(values[0]+"%");
            }

            @Override
            protected void onCancelled() {
                super.onCancelled();
            }

            @Override
            protected byte[] doInBackground(String... params) {
                String path = params[0];
                try{
                    // byte[] data = ImageService.getImage(path)
                    URL url = new URL(path);
                    HttpURLConnection conn = (HttpURLConnection)url.openConnection();
                    conn.setRequestMethod("GET");
                    conn.setConnectTimeout(5 * 1000);
                    long length = conn.getContentLength();
                    InputStream inStream = conn.getInputStream();
                    ByteArrayOutputStream outStream = new ByteArrayOutputStream();
                    byte[] buffer = new byte[1024];
                    int len = 0;
                    int count = 0;
                    while((len=inStream.read(buffer)) != -1){
                        outStream.write(buffer,0,len);
                        count += len;
                        if(length > 0){
                            publishProgress((int)((count/(float)length)*100));
                        }
                        Thread.sleep(100);
                    }

                    inStream.close();
                    return outStream.toByteArray();

                }catch (Exception e){
                    Toast.makeText(MainActivity.this,R.string.error,Toast.LENGTH_LONG).show();
                    Log.e(TAG,e.toString());

                }
                return null;
            }
        }
    }

---

## test1经验

- 
