# work or interview

## chrome webui工作

 content/public/browser/content_browser_client.h
  ... ::GetAdditionalWebUISchemes
//通过这个函数添加其他scheme(scheme://)处理
 
 ``` cpp
//下面这两个函数会调用web_ui_controller_factotory相关接口
  void NavigationRequest::CreateWebUIIfNeeded
  void RenderFrameHostImpl::SetWebUI(NavigationRequest& request)

 content/public/browser/web_ui_controller_factory.h
  virtual std::unique_ptr<WebUIController>
  WebUIControllerFactoryRegistry::CreateWebUIControllerForURL(WebUI* web_ui,
                                                              const GURL& url) = 0 
  virtual WebUI::TypeID WebUIControllerFactoryRegistry::GetWebUIType(
      BrowserContext* browser_context,
      const GURL& url) = 0
  virtual std::unique_ptr<WebUIController> CreateWebUIControllerForURL(
        WebUI* web_ui,
        const GURL& url) = 0
```

## ozone dfb


## ssl

客户端证书处理(选择)
支持wss的客户端证书处理

## web storage

localstorage 相关处理
indexdb

## v8

 ``` cpp
//v8初始化相关
third_party/blink/renderer/bindings/core/v8/v8_initializer.cc

//创建并且初始化 workerv8 isolate
third_party/blink/renderer/core/workers/worker_backing_thread.cc
::InitializeOnBackingThread ... 

//创建并且初始化main thread isolate
third_party/blink/renderer/controller/blink_initializer.cc
void Initialize(Platform* platform,
                mojo::BinderMap* binders,
                scheduler::WebThreadScheduler* main_thread_scheduler) {
  DCHECK(binders);
  Platform::InitializeMainThread(platform, main_thread_scheduler);
  InitializeCommon(platform, binders);
  V8Initializer::InitializeMainThread();
}
```

## cc

---

## viz

---

## net

---

## 性能优化

官方pgo开启方法
https://chromium.googlesource.com/chromium/src.git/+/master/docs/pgo.md

---

## devtools

通过这个继承这个类中的接口获取devtools的首页

``` cpp
//content/public/browser/devtools_manager_delegate.h
std::string DevToolsManagerDelegate::GetDiscoveryPageHTML() {
  return ui::ResourceBundle::GetSharedInstance()
      .GetRawDataResource(IDR_DEVTOOLS_DISCOVERY_PAGE)
      .as_string();
}
```

---

## other

页面适配
js接口增加
网站权限列表对应的文件

``` components/content_settings/core/common/content_settings_types.mojom
