# 欢迎界面
- 界面显示三秒后消失，第一次使用界面会显示软件介绍
- 按下界面会跳过三秒直接进入
- 使用 getSharedPreferences("data", Context.MODE_PRIVATE) 保存数据
- **使用 getdPreferences 保存的数据只能在本类中使用**
- **使用 getSharedPreferences 保存的数据可以在本软件中使用**

----
    private  int time = 3;

    final Handler handler = new Handler(){
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what){
                case 1:
                    time--;
                    Log.e("TAG",time+"");
                    if(time > 0){
                        handler.sendMessageDelayed(handler.obtainMessage(1),1000);
                    }
                    break;
            }
            super.handleMessage(msg);
        }
    };

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.welcome);

        handler.sendMessageDelayed(handler.obtainMessage(1),1000);

        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                startMainActivity();
            }
        },3000);
    }

    private  void startMainActivity(){
        // SharedPreferences sp = getPreferences(MODE_PRIVATE); 只在本类中使用
        SharedPreferences sp = getSharedPreferences("data", Context.MODE_PRIVATE);
        boolean isFirstUse  = sp.getBoolean("isFirstUse",true);
        if(isFirstUse){
            Intent intent = new Intent(WelcomActivity.this,beidai.introduce.Introduce.class);
            startActivity(intent);
            finish();
        }else{
            Intent intent = new Intent(WelcomActivity.this,MainActivity.class);
            startActivity(intent);
            finish();
        }
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if(event.getAction() == MotionEvent.ACTION_DOWN){
            startMainActivity();
            return true;
        }
        return super.onTouchEvent(event);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        handler.removeCallbacksAndMessages(null);
    }
