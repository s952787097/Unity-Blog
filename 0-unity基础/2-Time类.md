1.Time.timeScale
    传递时间的缩放。这可以用于减慢运动效果。
        当timeScale传递时间1.0时和实时时间一样快。当timeScale传递时间0.5时比实时时间慢一半。
	当timeScale传递时间为0时游戏基本上暂停了，如果你的所有函数是和帧速率无关的。
    除了realtimeSinceStartup，timeScale影响所有时间和增量时间基于Time类的变量。

    FixedUpdate functions will not be called when timeScale is set to zero.
    当timescale设置为0时，FixedUpdate函数将不会被调用。

     timeScale不影响Update和LateUpdate，会影响FixedUpdate
     timeScale不影响Time.realtimeSinceStartup，会影响Time.timeSinceLevelLoad和Time.time
     timeScale不影响Time.fixedDeltaTime和Time.unscaleDeltaTime，会影响Time.deltaTime
 
  

    Unity3D的Time.timeScale
   （1）Time.timeScale = 0可以暂停游戏，Time.timeScale = 1恢复正常，但这是作用于整个游戏的设置，不单单是当前场景，记得在需要的时候重置回Time.timeScale = 1。当然也可以使用Time.timeScale来做游戏的1倍、2倍整体加速。

   （2）timeScale影响的因素：

        设置Time.timeScale = 0 将会暂停所有和帧率无关的事情。这些主要是指所有的物理事件和依赖时间的函数、刚体力和速度等，而且FixedUpdate会受到影响，会被暂停（不是Update）,即timeScale =0 时将不会调用FixedUpdate函数了。
        但是，Update和LateUpdate函数本身的执行是不会受Time.timeScale的影响的。Update是依赖你的机器的，它的调用次数和你的机        器渲染一样快慢（一些特殊情况除外）；性能高的机器，帧率高，Update函数执行次数也就多。因此，当使用Time.timeScale = 0        时，所有和时间有关的事情都被暂停了。因为Time.timeScale为0时，Time.deltaTime将为0。这意味着，如果你使用        Time.deltaTime来控制旋转和位移等，那么Time.timeScale = 0也将使这些物体停止运动。所以游戏看起来是被冻结了，但是，我        们的游戏仍在渲染，也就是说Update函数仍在执行。

        所以Update和LateUpdate这两个方法里面需要自己用代码控制，且可以设定某些地方不会暂停（比如UI动画），弊端是如果想暂停，所有动画相关都要与Time.deltaTime这个字段绑定。

    （3）如果游戏暂停以后想在暂停界面上继续播放一些不受Time.timeScale 影响的动画或者粒子特效之类的，那么我们就需要用到         Time.realtimeSinceStartup去单独恢复他们，还有声音部分也需要单独恢复timeScale。
    （4）以前看到一种实现暂停的方法是放弃Time.deltaTime，自己实现两个函数：OnPauseGame和 OnResumeGame。这种方法扩展性强，缺点是写起来可能比较繁琐

2.Time.realtimeSinceStartup
    以秒计，自游戏开始的实时时间（只读）
	realtimeSinceStartup返回时间自由行开始，不被Time.timeScale影响。即使被玩家暂停realtimeSinceStartup也将保持增加（在后台）。当你想通过设置Time.timeScale为0暂停游戏时，使用realtimeSinceStartup是很有用的，但仍希望能够以某种方式来计算时间。即使场景退出后重新加载，也不受影响。

3.Time.deltaTime
    每秒一个单位的效果和每帧一个单位的效果实现
    每秒一个单位：transform.Translate(1*Time.deltaTime,0,0);
                  每秒一个单位就乘上 Time.delatTime;
    每帧一个单位：transform.Translate(1,0,0);
    
4.Time.time 当前帧的开始时间，这个时间是相对于程序开始的时间，所以在start函数里获取时这个值通常0；

5.如果需要获取系统时间，则需要用到C#提供的System.Date类的一些静态属性；

