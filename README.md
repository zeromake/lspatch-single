用于某些神秘的使用了 `LSPatch` 内置模块破解软件提取了模块也没有办法安装，独立生成模块 apk 的模板

## 需要工具

- android sdk
- [Apktool](https://github.com/iBotPeaches/Apktool)


**AndroidManifest.xml模板**

``` xml
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:compileSdkVersion="34"
    android:compileSdkVersionCodename="14"
    android:installLocation="internalOnly"
    package="${模块包名}"
    platformBuildVersionCode="34"
    platformBuildVersionName="14">
    <application
        android:allowBackup="false"
        android:extractNativeLibs="false"
        android:label="${模块名}"
        android:multiArch="true"
        android:supportsRtl="true">
        <meta-data android:name="xposedmodule" android:value="true"/>
        <meta-data
            android:name="xposedmodule"
            android:value="true" />
        <meta-data
            android:name="xposeddescription"
            android:value="${模块描述}" />
        <meta-data
            android:name="xposedminversion"
            android:value="54" />
        <meta-data
            android:name="xposedscope"
            android:value="${要生效对应的包名}" />
    </application>
</manifest>
```

## 步骤

1. 直接用 7zip 打开 apk 把 `lspatch/modules` 里的 apk 提取出来, 还有 `assets/lspatch/origin.apk` 提取出来直接安装上（这个一般就是原始包）
2. 使用 `java -jar apktool.jar d -o module-decoding module.apk` 把解密后的目录打开
3. 把这个上面的 `AndroidManifest.xml` 模板保存到 `module-decoding/AndroidManifest.xml`, 
4. 把 `apktool.yml` 里的 `packageInfo.forcedPackageId: 127` ，`sdkInfo.minSdkVersion: 27`
5. 使用 `java -jar apktool.jar b -o module-single.apk module-decoding`
6. 找个签名 apk 的工具签名 `module-single.apk` 最后就可以安装到自己手机上使用 `LSPosed` 来修改原始包了

## 测试情况

某箱的一些破解版测试情况：3.25.2 生效，3.25.4 的模块不能生效但是拿 3.25.2 的模块可以解决 3.25.4……
