最近的项目和一些图像处理有关，需要用C、C++实现，生成so文件，再通过JNI结合到Android的app中，有时候项目需要还会查看android的源码，做些调整，也会涉及到许多so文件，了解了一些JNI的技术。并且，正在读的一本书叫《深入理解Android》卷一，作者：邓平凡。该书是写的深入浅出，作者功力深厚，大力推荐购买。本文关于JNI的技术大部分参考该书第二章的内容，有兴趣的同学可以购买该书查看原文，这里作为我个人关于JNI的知识整理。

在进入正题之前，需要读者了解一些预备知识，比如关于JNI环境的配置，第一个jni程序hello-jni实现，具体参考：<http://whbzju.github.io/blog/2013/06/01/android-jni-config/>

#内容概述
本文从以下4个部分进行：

1. Java层，声明、使用native方法
2. Java与Native如何关联，即注册的方式与实现
3. Java与Native方法通信，即如何互相调用
4. Java与Native的数据结构对应关系

我相信，如果你弄懂了以上的问题，可以使用jni技术进行基本的开发。本文通过实现一个简单的demo，对以上的问题的进行解答。

#Java层---声明、使用native方法
先看MediaScanner.java的代码

{% codeblock MediaScanner lang:java %}

public class jniActivity extends Activity {

    private TextView tv;

    /**
     * Called when the activity is first created.
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.main);

        TextView  tv = new TextView(this);
        tv.setText( stringFromJNI() );
        setContentView(tv);
    }

    // 这个方法采用静态注册，参考ndk自带的例子hello-jni的实现
    public native String  stringFromJNI();

    // 修改成动态注册
    public native String DynamicStringFromJNI();

    // 提供方法，让native层调用
    public void testMethodForNativeCallJava(){
        Toast.makeText(getApplicationContext(), "Call from Native", Toast.LENGTH_SHORT).show();
    }

    // 加载jni库
    static{
       System.loadLibrary("learnJni");
    }
}

{% codeblock %}

可以大致猜到，static块load了jni的库文件，有好几个带native声明的方法，这些方法都没有具体的实现，其实现应该在native层也就是c层。该方法对java来讲使用起来没什么不同。Java层需要做的事情就结束了，秘密应该就在这两个我们不熟悉的部分。总结下Java层的工作：

* 通过static块来加载对应的jni库
* 声明由关键字native修饰的方法

看来Jni对Java程序员还是很友好的，使用起来很方便。

#Java与Native如何关联---注册JNI方法

首先，我们看下最简单的native方法实现，修改自ndk自带的sample：hello-jni的hellojni.c

{% codeblock learnjni lang:java %}
#include <string.h>
#include <jni.h>

/* This is a trivial JNI example where we use a native method
 * to return a new VM String. See the corresponding Java source
 * file located at:
 *
 *   apps/samples/hello-jni/project/src/com/example/hellojni/HelloJni.java
 */
jstring
Java_com_example_learnjni_LearnJni_stringFromJNI( JNIEnv* env,
                                                  jobject thiz )
{
    return (*env)->NewStringUTF(env, "Hello from JNI !");
}
{% codeblock %}






