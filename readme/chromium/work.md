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

---

### 页面适配

---

### js接口增加

---

网站权限列表对应的文件
components/content_settings/core/common/content_settings_types.mojom

---

### coredump问题

90版本webview会遇到这样的coredump
GrBackendTexture::~GrBackendTexture()
SkPromiseImageTexture::~SkPromiseImageTexture()
gpu::SharedImageRepresentationSkiaGL::~SharedImageRepresentationSkiaGL()
gpu::SharedImageRepresentationSkiaGL::~SharedImageRepresentationSkiaGL()
viz::ImageContextImpl::~ImageContextImpl()

https://skia-review.googlesource.com/c/skia/+/375201
https://skia-review.googlesource.com/c/skia/+/487382

chromium修复了很多部分悬空指针的问题在这个问题单
部分是用raw_ptr规避 90上还未启动
可以把不使用raw_ptr 修复的方法合入
https://bugs.chromium.org/p/chromium/issues/detail?id=1291138

DanglingPtr: fix dangling ptr in SharedImageRepresentation:
https://chromium-review.googlesource.com/c/chromium/src/+/3827433

quic相关的patch

5570148: Fix DanglingUntriaged pointer in QuicChromiumPacketWriter | https://chromium-review.googlesource.com/c/chromium/src/+/5570148 (后续再合入)
5658194: Fix dangling raw_ptrs in QuicChromiumClientSession | https://chromium-review.googlesource.com/c/chromium/src/+/5658194 (这笔没走到相关逻辑 不合入)
5570148: Fix DanglingUntriaged pointer in QuicChromiumPacketWriter | https://chromium-review.googlesource.com/c/chromium/src/+/5570148 (后续再合入)

这两笔有所关联
4570128: Notify QuicPacketWriter that socket has closed | https://chromium-review.googlesource.com/c/chromium/src/+/4570128
4705611: Add tests for connection migration failure race. | https://chromium-review.googlesource.com/c/chromium/src/+/4705611

5570148: Fix DanglingUntriaged pointer in QuicChromiumPacketWriter | https://chromium-review.googlesource.com/c/chromium/src/+/5570148

5519211: Remove DanglingUntriaged annotation from QuicClientSession | https://chromium-review.googlesource.com/c/chromium/src/+/5519211

这种使用raw_ptr修复的等后续chromium修复后再更改
DanglingPtr: Fix dangling ptr in HttpStreamRequest
https://chromium-review.googlesource.com/c/chromium/src/+/3780168

### android字体相关

android 默认会获取 $\underline{/system/fonts/}$ 路径下面的fonts.xml以及字体
<u>src/third_party/skia/src/ports/SkFontMgr_android.cpp<u>
