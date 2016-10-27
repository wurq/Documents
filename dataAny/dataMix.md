# 数据埋点分析

## 一  数据埋点的需求
a 分析用户行为，用数据说话来优化用户体验
数据采集  数据传输  数据建模、存储    数据统计、分析  数据可视化、反馈

终端需要着重解决的是数据采集和数据传输的问题

##二 数据埋点的常见思路分析
###1  代码埋点
      Senors Analytics umeng
      埋点准确性高， 但是工作量大需要不断更新
###2  可视化埋点，无需代码埋点
由于可视化埋点的局限性，需要一些新的思路——— 可视化埋点，也叫无代码埋点

可以通过后台配置的方式  无需处处加入代码
代表：  mix panel， 诸葛IO，Talking Data

##三  mix panel数据埋点的思路分析
1 token / id login
2 跟踪事件分析track event, ， 用户分析 update people analysis record
3 事件不会被即时发送到服务器
4 考虑应用本身的生命周期，定期的发送数据
5 superProperties的添加

##四 可视化埋点的主要思路  android
###1 组件的运行监控，涉及用户行为的主要是Activity
1.  Activity 生命周期的管理，可以监控界面
           使用 Application.ActivityLifecycleCallbacks 监控所有的Activity的全部生命周期，
2. 获得顶层rootView 用户行为的监控
           对于用户注册用户的数据分析？？
           用户行为事件分析：  从 ViewTreeObserver.OnGlobalLayoutListener 出发 向下分解rootView各组件，
           
   基本思路:   
           
a. 获得顶层rootView
```
private static class XXXXX implements ViewTreeObserver.OnGlobalLayoutListener, Runnable 
```
b. 从rootView开始向下分解, 匹配各级view数据, 已经匹配好的各级view 建立索引(index)数据, 发送给后台
c. 从后台获得需要的view 以及相应的需要采集的event事件列表 
d. 得到匹配后的view 以及event列表,将数据存储,并且监控。
```
 View.AccessibilityDelegate ret = null;
            try {
                Class<?> klass = v.getClass();
                Method m = klass.getMethod("getAccessibilityDelegate");
                ret = (View.AccessibilityDelegate) m.invoke(v);
            } 
            
            
            final TrackingAccessibilityDelegate newDelegate = new TrackingAccessibilityDelegate(realDelegate);
                        found.setAccessibilityDelegate(newDelegate);
```



###2 可视化
      a  用户界面截图以及界面分解
      b  从顶view向下分解的过程中监控相应的event

###3 网络传输
      a  事件日志存储管理
      b  缓存延后传输机制

###4 考虑部分数据会在app关闭时传输，网络独立后台推送机制？