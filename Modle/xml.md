# android界面设计

## 使用Design图形界面设置界面

- 先设置布局方式，再设置布局里的一些排列
- TextView、EditText的id名命名tv... ,et...等
- Design界面设置不了布局详细信息，需要到Text界面编辑

## 界面间切换

-  按键监听切换界面

        // 从Login.class 通过按键 btnReg 切换到RegActivity.class
        Button btnReg = (Button)findViewById(R.id.btnReg);
        btnReg.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(LoginActivity.this,com.example.beidai.myapplication.RegActivity.class);
                startActivity(intent);
                finish();
            }
        });

        // 在RegActivity.class 设置 reg 界面
        public class RegActivity extends AppCompatActivity {
            @Override
            public void onCreate(final Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.reg);
              }
            }

- 重写onDestroy方法，可在界面关闭后执行操作

      @Override
      protected void onDestroy(){
        if(mc != null){
            mc.sayBye();
        }
        super.onDestroy();
      }

-  通过同一个类里的handle消息机制显示隐藏界面

        // 接收消息，如果接收到消息为 0 则切换界面
        Handler myHandler = new Handler(){
            @Override
            public void handleMessage(Message msg){
                switch (msg.what){
                    case 0:
                        View linearLayout = (LinearLayout)findViewById(R.id.regResult);
                        linearLayout.setVisibility(View.VISIBLE);
                        EditText etUno = (EditText)findViewById(R.id.etUno);
                        etUno.setText(uno);
                        break;
                }
                super.handleMessage(msg);
            }
        };

        // 满足条件，发送消息 0
        myHandler.sendEmptyMessage(0);

## 判断账号密码

- 账号密码为空，返回继续判断

      EditText etUid = (EditText)findViewById(R.id.etUid);
      EditText etPwd = (EditText)findViewById(R.id.etPwd);
      String uid = etUid.getEditableText().toString().trim();
      String pwd = etPwd.getEditableText().toString().trim();
      if(uid.equals("") || pwd.equals("")){
          Log.i("AppDebug","Toast");
          return;
      }

## 对话框和Toast提示以及调试信息

- 设置一个对话框

      ProgressDialog pd;
      // 可以返回退出
      pd = ProgressDialog.show(LoginActivity.this,"请稍后","正在连接服务器",true,true);
      // 不可以返回退出
      pd = ProgressDialog.show(RegActivity.this,"请稍后","正在连接服务器...",false);

      pd.dismiss(); // 关闭对话框

- Toast提示

      Toast.makeText(RegActivity.this,"请将注册信息填写完整",Toast.LENGTH_LONG).show();

- 调试信息

      Log.i("AppDebug","请将注册信息填写完整");

##
