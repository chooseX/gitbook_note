# AssociatedInterfaceRegistry用法

chromium renderframe层的使用

***content/public/browser/render_frame_host.h*** 和 ***content/public/renderer/render_frame.h***
都实现了这个接口 

```cpp
virtual blink::AssociatedInterfaceProvider* GetRemoteAssociatedInterfaces() = 0; 
```

首先 receive端通过AddInterface添加对应callback到bindmaps
remote端使用AssociatedInterfaceProvider提供的接口就可以通过mojo ipc触发TryBindInterface接口


