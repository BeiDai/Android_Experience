# 账号注册界面

- **判断是否为空和密码是否相同**
---
    if(name.equals("")||email.equals("")||password.equals("")||passwordVerity.equals("")){
        Toast.makeText(register.this,"请将注册信息填写完整",Toast.LENGTH_LONG).show();
        return;
    }

    if(!password.equals(passwordVerity)){
        Toast.makeText(register.this,"两次输入的密码不一致",Toast.LENGTH_LONG).show();
        return;
    }
- **使用SharedPreferences存取数据**
---
    public void rememberRegister(String name,String email,String password){
        SharedPreferences sp = getSharedPreferences("data", Context.MODE_PRIVATE);
        SharedPreferences.Editor editor = sp.edit();
        editor.putString("name",name);
        editor.putString("email",email);
        editor.putString("password",password);
        editor.putBoolean("isFirstUse",false);
        editor.commit();
    }
