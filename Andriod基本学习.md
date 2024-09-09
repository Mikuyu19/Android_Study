# Android初步学习

从今天开始初略的学习关于一些Android的相关知识
//2024/7/13
主要的话还是学习关于一些方法的使用
对于那些不太熟悉的

此篇文章的学习是在该视频下学习

【手把手教你用Android Studio写一个APP】 https://www.bilibili.com/video/BV1MK411p7dp/?share_source=copy_web&vd_source=4cfc90c13cf1995225f158ccd5b28864

其他的学习路径可参考【【天哥】Android开发视频教程最新版 Android Studio开发】 https://www.bilibili.com/video/BV1Rt411e76H/?share_source=copy_web&vd_source=4cfc90c13cf1995225f158ccd5b28864

## 一.LinearLayout与RelativeLayout

### 1、排列方式

android:orientation="vertical" //用来显示textview 的排列顺序
vertical 表示竖直排列
horizontal 表示水平排序

//2024/9/5继续更新
在mainactivity.xml
    android:layout_width="match_parent"
    android:layout_height="match_parent"
	匹配父部布局
	或有wrap_parent有多高显示多高
	
改完LinearLayout后加上orientation确定其排列方式
控件距离：padding：10dp   代表前后左右都是10dp的空间距离
如果想修改就别的就加上left之类的方位词

### 2、制作一个简易的登录界面

（1）、学会使用textview 、button、edittext--->简易的登录界面
	文本颜色textColor  #000000代表黑色
	文本大小textSize #18sp  采用sp来作为单位
	文本行数maxEms：10   显示一行最多显示多少个字符
		   maxLines 最大行数可以显示多少行
		   ellipsize：end 省略号表示
		

	Layout_gravity控件位置
	gravity单独位置

（2）使用Edittext
        

```
android:id="@+id/et_1"
android:layout_width="match_parent"
android:layout_height="50dp"
android:textSize="16sp"
android:textColor="@color/black"
android:hint="用户名"		//显示的内容
android:maxLines="1"		//最多显示行数
android:padding="5dp"		//与周边的控距
android:inputType="textPassword"	//输入显示密码不可见
```

​	不多介绍，与上面的差不多
（3）Button使用

```
    <Button
        android:id="@+id/Btn_login"		//修改id
        android:layout_width="200dp"	//控件button按钮的大小
        android:layout_height="wrap_content"	
        android:layout_gravity="center"
        android:layout_marginTop="20dp"	//距离顶部
        android:background="#7774D6"	//背景颜色 
        android:text="登录"
```

如果遇到背景颜色无法修改的情况这是由于新版本的主题问题导致的
解决办法:修改app/res/values目录下的themes.xml,改为我如图所示的代码

```
  <style name="Theme.Test1" parent="Theme.MaterialComponents.DayNight.NoActionBar.Bridge">
```

//2024/9/7

## 二.学习界面优化

在drawable新建了三个文件bg_username
在第一个username——bg里面写了一个控件外形

```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">	//shape代表形状绘制
    <stroke				//stroke形状的笔划中线。
        android:width="1dp"
        android:color="@color/white"
    />
<corners			//corners为形状产生圆角。仅当形状为矩形时适用。
    android:topLeftRadius="5dp"
    android:bottomLeftRadius="5dp"

    /></shape>
```


还是很好理解的

//2024/9/8

## 三.实现具体功能button跳转

### （1）、两个activity跳转

匹配对应用户名与密码
获取edittest里面的用户名和密码
与规定进行匹配
成功则进行跳转

### （2）、跳转前界面-->跳转后界面

//声明控件

```
private Button mBtnlogin;
```

//找到控件

```
mBtnlogin = findViewById(R.id.Btn_login);
```

//实现直接跳转 ---方法1

```
mBtnlogin.setOnClickListener(new View.OnClickListener() {	//设置点击按钮事件
	@Override
	public void onClick(View view) {
		Intent intent = null;	//Intent代表意图指向空
		intent = new Intent(MainActivity.this, FunctionActivity.class);//一个事件活动指向另一个事件活动
		startActivity(intent);//启动
	}
});
```

方法二-//实现匹配对应的用户名和密码才能进行登录操作
        
		
声明控件

```
    private EditText mEtuser;
    private EditText mEtpassword;
```

找到控件

```
	mEtuser = findViewById(R.id.et_1);
	mEtpassword = findViewById(R.id.et_2);
```

实现调用onClick方法

```
mBtnlogin.setOnClickListener(this::onClick);
```

​	

```
 public void onClick(View v){
            //获取需要输入的用户名和密码
            String username = mEtuser.getText().toString(); //获取输入的信息存入字符串
            String password = mEtpassword.getText().toString();//同理
            Intent intent = null;
            //假设正确的用户名和密码是Miku ，123456
            if(username.equals("Miku")&&password.equals("123456")) {
                //如果正确的话就进行跳转
                intent = new Intent(MainActivity.this, FunctionActivity.class);
                startActivity(intent);
            }
            else{
                //弹出登录失败
            }
    }
```






需要public class MainActivity extends AppCompatActivity implements View.OnClickListener
然后重写   

```
 @Override
    public void onPointerCaptureChanged(boolean hasCapture) {
        super.onPointerCaptureChanged(hasCapture);
    }
```

因为继承了View.OnClickListener





## 四.实现toast跳转弹窗

### 1、底部弹出

```
//弹出的登录设置
String ok = "登录成功!";
String fail = "密码或账号有误，请重新登录！";
```

```
Toast.makeText(getApplicationContext(),ok,Toast.LENGTH_SHORT).show();
```

直接使用此句就行

### 2、居中弹出

```
//弹出登录失败
//Toast提升版本居中显示
Toast toastCenter = Toast.makeText(getApplicationContext(),fail,Toast.LENGTH_SHORT);
toastCenter.setGravity(Gravity.CENTER,0,0); //设置居中
toastCenter.show();
```

此版本在Android11以后好像不适用了，好像在AS还是会显示为底部弹出，需要修改的去CSDN上找找如何修改为居中吧。

### 3、封装类弹出

首先在com.example.test1路径下新建一个Util包

新写一个类，名为ToastUtil

```
package com.example.test1.util;

import android.content.Context;
import android.widget.Toast;

//Toast弹窗封装代码

public class ToastUtil {
    public static Toast mToast;
    public static void showMsg(Context context,String msg){
        if(mToast == null){
            mToast = Toast.makeText(context,msg,Toast.LENGTH_SHORT);
        }
        else{
            mToast.setText(msg);
        }
        mToast.show();
    }
}
```

使用方法如下

直接调用改类即可

```
//弹出登录成功
ToastUtil.showMsg(getApplicationContext(),ok);
//同样采用ToastUtil,这里就不居中显示了
ToastUtil.showMsg(MainActivity.this,fail);
```

这里的getApplicationContext()可以修改为MainActivity.this。

在此，登录操作的优化就差不多了。

