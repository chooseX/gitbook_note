# work or interview

## chrome webui工作

 content/public/browser/content_browser_client.h
  ... ::GetAdditionalWebUISchemes ...
  通过这个函数添加其他scheme(scheme://)处理
 
 下面这两个函数会调用web_ui_controller_factotory相关接口
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

### ozone dfb

# ssl

# web storage

# v8

# cc

# viz

# net

