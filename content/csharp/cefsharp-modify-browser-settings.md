---
title: "Modifying Browser Settings with CefSharp"
---


```csharp
using CefSharp;
using CefSharp.Wpf;
using Newtonsoft.Json.Linq;
using System.ComponentModel;

namespace YRC
{
    class Browser
    {
        public static void Init(System.Windows.Controls.Grid container)
        {
            // 初始化浏览器
            var settings = new CefSettings();
            settings.CefCommandLineArgs.Add("autoplay-policy", "no-user-gesture-required");
            Cef.Initialize(settings);
            ChromiumWebBrowser chromium = new ChromiumWebBrowser();
            container.Children.Add(chromium);

            // 设置事件
            chromium.FrameLoadEnd += OnBrowserFrameLoadEnd;

            // 加载网页
            var mac = MachineCode.GetMacAddress().Replace(":", "-");
            var url = $"http://10.0.0.58/#/?mac={mac}";
            chromium.Load(url);
        }

        private static void OnBrowserFrameLoadEnd(object sender, FrameLoadEndEventArgs args)
        {
            if (args.Frame.IsMain)
            {
                args.Browser.ShowDevTools();
                args.Browser.MainFrame.ExecuteJavaScriptAsync("document.body.style.overflow = 'hidden'");
                //args.Browser.MainFrame.ExecuteJavaScriptAsync("document.querySelectorAll('video').forEach(v => v.play())");
                //var js = @"var videoElements = document.querySelectorAll('video');
                //    for (var i = 0; i < videoElements.length; i++) {
                //        var video = videoElements[i];
                //        if (video.autoplay) {
                //            video.play();
                //        }
                //    }";
                //args.Browser.MainFrame.ExecuteJavaScriptAsync(js);
            }
        }
    }
}

```
