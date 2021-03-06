> 没有干货，只有湿货

不讲技术，只是一个程序员的吐槽，仅此而已。。

> 突然冒出了n多消息推送平台。

记得2年前，Android消息推送，就像雨后的春笋一样，冒出来了很多，主要以友盟，极光为代表，再然后BAT也陆续推出了自己的消息推送平台，可以说，这么多的消息推送平台，让咱们挑花了眼，各有各的优点，我也在当时问了下周围的朋友，用什么平台的都有。各大消息推送平台， 都想在这里分得一杯羹，就像当初的云盘一样，再看看现在的消息推送平台，要死不活的平台一堆。

> 杀不死的推送小强。

现在推送，基本上就是后台和服务器，保持一个长连接，如果后台有新的消息，需要推送到用户的手机上，只需通过这个长连接，就可以直接推送，这个长连接维持，在Android里面，咱们靠的是Service组件，所以说，这个Service起着至关重要的作用，如果我们要保证消息能够及时的推送到用户手机上，就必须保证这个Service的运行，为了Service的运行，咱们把能监听的系统广播（网络连接状态广播，开关屏广播，开机广播等），都给监听了，为的就是提高Service的存活率，说实话，这些广播足以维持Service的存活。

> 手机厂商祭出了屠龙刀！！

这样杀不死的推送小强，存在了好长一段时间，用户只要多开几个app，手机就开始发热，卡顿，于是各种卫士管家啥的都出来了，说是提供一键清理的功能，虽然当时似乎有那么点效果，但是过不了多久，手机又开始卡，发热，耗电等等一系列问题接踵而至，当然用户发现这种问题，第一个责怪的就是手机厂商，厂商成为了无辜的背锅侠，于是怒了，终于祭出了屠龙刀，手机提供一个系统级别的一键清理，只要不是白名单的程序，一律干掉，各大app尸首遍地，惨目忍睹。

> 道高一尺魔高一丈！！

当然咱们程序员也不是吃素的，于是推出了各种保活策略，例如：多进程守护，native层直接fork一个进程等等所谓的黑科技，有的效果还不错，在部分的国内厂商的手机上，还真有效果，不过现在看看，所谓的保活黑科技还真的有效吗？据我现在的测试，目前在小米，华为，魅族手机上，所谓的黑科技都已经失效了，咱们程序员和厂商的斗智斗勇，最后还是以厂商的胜利告终，道可道，非常道！

> 哥俩好，手拉手往前走。。

记得一次，产品经理说：“为啥人家微信QQ，怎么都能收到推送消息，咱们的就不可以！”，当时心里成千上万的草泥马在奔腾，你说咱们程序员咋就这么苦逼，不就一个消息推送吗，搞得人要死要活的，谁叫咱们的Google大爹，不硬气呢，凡是使用Android系统的必须集成Google服务，那咱们岂不爽歪了，咱们android也可以像Apple一样，那叫一个简单，想想都觉得美，算了，别做梦了，咱们帝国还有一堵高墙。不过，曙光即将到来，那就是厂商为了解决这种相爱相杀的矛盾，终于出了自己的推送，而且还是系统级别的，记得小米推送出来的时候高兴了半天，终于不用担心小米的一键清理了，随后各大厂商的推送陆续的出来了，像是华为，魅族都有自己的推送。

> 到底有没有好的推送解决方案呢？

说实话，这个问题困扰了我很久，不能等着产品骂你不行，而你又无动于衷，于是开始查找一些关于消息推送的解决方案，当时也发现了一个解决方案，那就是阿里云推，它可以提供一个辅助推送通道，不过仔细看了下文档，还是发现存在一些问题的。

![阿里云推-辅助推送通道.png](http://upload-images.jianshu.io/upload_images/1460021-ec49d6cdb293d77a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
看到阿里的这个说明，咱得先明白两个词“通知”和“透传”，由于阿里的辅助通道使用的透传，而透传在app被一键清理掉，那就收不到透传，但是通知，小米和华为却是以系统服务维持的，就算app被清理掉，手机还是能接收到通知的，所以说，阿里的这个辅助通道，也不能完全保证消息的送达。既然通知是系统级维护的，那么根据不同的手机型号，集成不同的消息推送，通过厂商平台，发送通知，这不就可以大大提高消息推送达率的，于是就诞生了[OnePush](https://github.com/pengyuantao/OnePush)。

优势对比

| |OnePush|非厂商推送|厂商推送|辅助通道推送|
|---------|:-------:|:-------:|:-------:|:-------:|
|存活率|高|低|高|低|
|送达率|厂商高，其他根据当前系统|中|手机区分|中|
|拓展性|强|低|低|低|

目前[OnePush](https://github.com/pengyuantao/OnePush)，提供了小米和华为的推送拓展，你可以自由拓展其他的消息推送平台，当然你也不用拓展其他的消息推送，完全使用小米和华为的拓展也是可以的，华为平台使用华为推送，其他平台使用小米推送，小米推送完全可以替代其他的非厂商推送，为啥呢，因为小米推送，不仅支持小米手机，还支持其他手机，当然在其他手机上不是系统级别的服务而已。

> 推广一下[OnePush](https://github.com/pengyuantao/OnePush)

附上OnePus开源地址：  https://github.com/pengyuantao/OnePush

喜欢就star下，说不定能用着，吐槽完了，这么多文字，能看到最后，你很有前途！