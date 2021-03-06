---
title: Gradle相关配置
date: 2016-09-09 10:36
categories: Android 
tags: Gradle
---
```
android {
    compileSdkVersion 24
    buildToolsVersion "24.0.0"

    defaultConfig {
        applicationId "com.geng.hello"
        minSdkVersion 19
        targetSdkVersion 24
        versionCode 1
        versionName "1.0"
        // 在buildConfig.java中自动配置常量字段
        buildConfigField('String', "API_DEBUG_URL", '"http://test.baidu.com"')

        // 开启multidex,突破64k
        multiDexEnabled true

        // ndk cpu架构，生成相应的so包
        ndk{
            abiFilter 'armeabi', 'x86', 'armeabi-v7a', 'x86_64', 'arm64-v8a'
        }
    }

    // 渠道
    productFlavors {
    }

    signingConfigs {
        relase {
            storeFile file("relase.keystore")
            keyAlias "relase"
            keyPassword "123456"
            storePassword "123456"
        }
        debug {
        }
    }

    buildTypes {
        release {
            // 混淆开启
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        debug {
            minifyEnabled false
            // 4byte对齐，加快解析效率
            zipAlignEnabled true
            // 移除无用的资源文件
            shrinkResources true
            // 更改包名
            applicationVariants.all { variant ->
                variant.outputs.each { output ->
                    output.outputFile = new File(output.outputFile.parent,
                            "debug_" + buildTime() + ".apk")
                }
            }
        }
    }

}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:24.0.0'
}



// --------------------------------gradle.properties配置------------------------------

// 加速构建速度
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.jvmargs=-Xmx2048m

// 使用ndk配置
android.useDeprecatedNdk=true


// 其他：
1.查看 keystore 文件的签名信息：

在keystore所在目录下，打开cmd:
keytool -list -v -keystore keystore文件名

再根据提示，输入storepassword即可查看

2.混淆：
https://segmentfault.com/a/1190000004461614
http://www.mayflyask.org/question/247
```