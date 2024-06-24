# AssociatedInterfaceRegistry用法

chromium renderframe层的使用

_**content/public/browser/render\_frame\_host.h**_ 和 _**content/public/renderer/render\_frame.h**_ 都实现了这个接口

```cpp
virtual blink::AssociatedInterfaceProvider* GetRemoteAssociatedInterfaces() = 0; 
```

首先 receive端通过AddInterface添加对应callback到bindmaps remote端使用AssociatedInterfaceProvider提供的接口就可以通过mojo ipc触发TryBindInterface接口
