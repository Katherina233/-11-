# -11-
 关于序列化和反序列化
序列化:将对象转化成流的过程称为序列化

反序列化:将流转化成对象的过程称之为反序列化

序列化与反序列化必须遵守的原则

a)         Java对象

在java中要想使一个java对象可以实现序列化与反序列化,必须让该类实现java.io.Serializable接口

java.io.Serializable接口定义如下:

publicinterface Serializable {

}

从上述定义中可以看到该接口中未定义任何方法,这大大的简化了开发者

b)      序列化主要依赖java.io.ObjectOutputStream类,该类对java.io.FileOutputStream进一步做了封装,这里主要使用ObjectOutputStream类的writeObject()方法实现序列化功能

Demo:

/**

     *将对象序列化到磁盘文件中

     *@paramo

     *@throwsException

     */

    publicstaticvoid writeObject(Object o) throws Exception{

       File f=new File("d:""user.tmp");

       if(f.exists()){

           f.delete();

       }

       FileOutputStream os=new FileOutputStream(f);

       //ObjectOutputStream 核心类

       ObjectOutputStream oos=new ObjectOutputStream(os);

       oos.writeObject(o);

       oos.close();

       os.close();

    }

c)      反序列化主要依赖java.io.ObjectInputStream类,该类对java.io.InputStream进一步做了封装,这里主要使用ObjectInputStream类的readObject()方法实现序列化功能

Demo:

/**

     *反序列化,将磁盘文件转化为对象

     *@paramf

     *@return

     *@throwsException

     */

    publicstatic User readObject(File f) throws Exception{

       InputStream is=new FileInputStream(f);

       //ObjectOutputStream 核心类

       ObjectInputStream ois=new ObjectInputStream(is);

       return (User)ois.readObject();

    }

贴出完整的demo

Java对象:

package com.io.bean;

import java.io.Serializable;

publicclass User implements Serializable {

   

    privateintuserId;

    private String userName;

    private String userSex;

    privateintuserAge;

    publicint getUserAge() {

       returnuserAge;

    }

    publicvoid setUserAge(int userAge) {

       this.userAge = userAge;

    }

    publicint getUserId() {

       returnuserId;

    }

    publicvoid setUserId(int userId) {

       this.userId = userId;

    }

    public String getUserName() {

       returnuserName;

    }

    publicvoid setUserName(String userName) {

       this.userName = userName;

    }

    public String getUserSex() {

       returnuserSex;

    }

    publicvoid setUserSex(String userSex) {

       this.userSex = userSex;

    }

    @Override

    public String toString() {

       returnthis.getUserId() + "   " + this.getUserName() + "   "

              + this.getUserSex() + "    " + this.getUserAge();

    }

}

序列化与反序列化

package com.io.test;

import java.io.File;

import java.io.FileInputStream;

import java.io.FileOutputStream;

import java.io.InputStream;

import java.io.ObjectInputStream;

import java.io.ObjectOutputStream;

import com.io.bean.User;

publicclass TestSerializable {

    /**

     *将对象序列化到磁盘文件中

     *@paramo

     *@throwsException

     */

    publicstaticvoid writeObject(Object o) throws Exception{

       File f=new File("d:""user.tmp");

       if(f.exists()){

           f.delete();

       }

       FileOutputStream os=new FileOutputStream(f);

       //ObjectOutputStream 核心类

       ObjectOutputStream oos=new ObjectOutputStream(os);

       oos.writeObject(o);

       oos.close();

       os.close();

    }

   

    /**

     *反序列化,将磁盘文件转化为对象

     *@paramf

     *@return

     *@throwsException

     */

    publicstatic User readObject(File f) throws Exception{

       InputStream is=new FileInputStream(f);

       //ObjectOutputStream 核心类

       ObjectInputStream ois=new ObjectInputStream(is);

       return (User)ois.readObject();

    }

   

   

    publicstaticvoid main(String[] args) throws Exception{

      

       /*****************将对象序列化***************/

      

    /* 

        User user=new User();

       user.setUserId(1);

       user.setUserName("张艺兴");

       user.setUserSex("男");

       user.setUserAge(50);

       TestSerializable.writeObject(user);

    */

      

       /*****************将对象序反列化***************/

      

       User user=TestSerializable.readObject(new File("d:""user.tmp"));

       System.out.println(user);

    }

}
