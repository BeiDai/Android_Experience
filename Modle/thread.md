# Thread建立线程处理任务


    new Thread(){
         // 线程执行任务
         public void run(){
                          // 循环执行
                          Looper.prepare();

                          // 处理耗时任务
                          try{
                                  // 耗时任务                 
                              }catch (Exception e){
                              e.printStackTrace();
                            }
                          }

                  // 开启线程
                  }.start();
