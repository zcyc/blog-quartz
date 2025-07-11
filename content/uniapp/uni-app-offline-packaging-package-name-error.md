---
title: "uni-app: Offline Packaging Error When Changing Package Name"
---


```xml
<!-- 如果你不修改启动文件，仅仅是修改包名。那么修改包名以后，要修改 android:authorities 字段为包名 -->
<provider
    android:name="io.dcloud.common.util.DCloud_FileProvider"
    android:authorities="io.dcloud.simple.dc.fileprovider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/dcloud_file_provider" />
</provider>
```
