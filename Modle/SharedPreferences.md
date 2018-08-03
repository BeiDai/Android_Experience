# SharedPreferences数据格式使用

- 写入账号密码

      public void rememberMe(String uid_data),String pwd_data)){
          SharedPreferences sp = getPreferences(MODE_PRIVATE);
          SharedPreferences.Editor editor = sp.edit();

          // 写数据到uid_data 到 标签 uid 中
          editor.putString("uid",uid_data);
          editor.putString("pwd",pwd_data));

          // 提交数据
          editor.commit();
      }

- 读取账号密码

      public void checkIfRemember(){
              SharedPreferences sp = getPreferences(MODE_PRIVATE);

              // 读取账号密码
              String uid_data = sp.getString("uid",null);
              String pwd_data = sp.getString("pwd",null);

              // 如果不为空，则自动填写账号密码
              if(uid_data != null && pwd_data != null){
                  EditText etUid = (EditText)findViewById(R.id.etUid);
                  EditText etPwd = (EditText)findViewById(R.id.etPwd);
                  CheckBox cbRemember = (CheckBox)findViewById(R.id.cbRemember);
                  etUid.setText(uid_data);
                  etPwd.setText(pwd_data);
                  cbRemember.setChecked(true);
              }
          }
