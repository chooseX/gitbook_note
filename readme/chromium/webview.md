# chromium webview

## 将webview改成aar需要处理的点

1. gn dist_aar 没有打包locales的资源
    build/android/gyp/write_build_config.py 需要做如下修改

    ``` python
      if os.environ.get('self_build') == 'true' and options.type == 'dist_aar':
        config['assets'], config['uncompressed_assets'], locale_paks = (
            _MergeAssets(deps.All('android_assets')))
        deps_info['locales_java_list'] = _CreateJavaLocaleListFromAssets(
            config['uncompressed_assets'], locale_paks)
    ```

2. dist_aar缺少标准aar的一些文件
chromium 90版本 dist_aar class缺少生成GEN_JNI和R  [相关改动](https://chromium-review.googlesource.com/c/chromium/src/+/3554371)
缺少snapshot_blob_xx.bin
缺少assets
缺少icu data
缺少pak reources
res生成名字也有错误
