---
layout: post
title:  "摇杆控制角色,按玩家屏幕方向移动,摄像机可自由调整"
date:   2017-08-05 19:05:13 +0000
category: tech
---

今天做游戏的时候,要测试美术场景,让我快速加一个摇杆控制.
摇杆操作的时候,要和玩家视角保持一致,而不是根据角色方向.
结果很简单的东西犯二居然搞了好一会.特此记一下.....


思路:
角色移动的平面,默认是XZ面.
虚拟摇杆操作的是屏幕,假设屏幕在世界中,则是一个法向量和XZ平面法向量垂直的面.
要达到标题的效果,就要让屏幕上摇杆的位移等效于XZ平面的位移.所以要用摄像机的坐标来做文章.
先通过摄像机转化,把屏幕转换到世界坐标系,这个时候再移动摇杆,得到的就是世界坐标系里的一个位移向量.
把这个位移向量投影到角色移动的ZX平面,则可以相对准确移动.角色越接近屏幕中心,越准确.



下面是代码:
这里move.joystickAxis就是已经获取好的摇杆相对位移向量Vector2

{% highlight c# %}
void OnJoystickMove(MovingJoystick move)
{        
    //获取摄像机在世界坐标的位置
    Vector3 cameraPos = Camera.main.ScreenToWorldPoint(Vector3.zero);
    Vector3 offsetPos = cameraPos;
    //在摄像机平面上,获取2D屏幕的移动转化为世界坐标后的点位置
    offsetPos += Camera.main.transform.right * move.joystickAxis.x;
    offsetPos += Camera.main.transform.up * move.joystickAxis.y;
    Vector3 dir = offsetPos - cameraPos;
    //投影到Actor移动的那个面上去,已知actor在XZ平面上,所以直接投影到XZ平面,因为是XZ面,投影很方便,直接Y归0
    dir = new Vector3(dir.x,0, dir.z).normalized;
    //移动
    agent.Move(dir * agent.speed * Time.deltaTime);
}
{% endhighlight %}

---
{% if site.intensedebate_comments %}
  {% include intensedebate-comments.html %}
{% endif %}

[how]: http://blog.slaks.net/2013-06-10/jekyll-endraw-in-code/

