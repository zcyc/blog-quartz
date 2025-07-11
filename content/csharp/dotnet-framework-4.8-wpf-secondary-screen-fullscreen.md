---
title: ".NET Framework 4.8 WPF: Start on Secondary Screen and Set to Fullscreen"
---


# 1、添加引用

在项目中添加以下引用

```
System.Windows.Forms
System.Drawing
```

# 2、移动窗口并修改属性

```csharp
using System.Linq;
using System.Windows;
using System.Windows.Forms;

namespace WpfApp2
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            // 移动到副屏
            var secondaryScreen = Screen.AllScreens.FirstOrDefault(s => !s.Primary);
            if (secondaryScreen == null) return;
            Left = secondaryScreen.Bounds.Left;
            Top = secondaryScreen.Bounds.Top;
            // 全屏
            Width = secondaryScreen.Bounds.Width;
            Height = secondaryScreen.Bounds.Height;
            // 置顶
            Topmost = true;
            // 禁止修改大小
            ResizeMode = ResizeMode.NoResize;
        }
    }
}
```

# 3、关闭窗口菜单栏

在 xaml 的 Window 节点下添加以下属性

```
WindowStyle="None"
```
