# Android_Gradle_DSL_4.1.0
com.android.tools.build:gradle:4.1.0

`Android Studio 4.1`支持在`Gradle`文件中, 跳转源码了,比原来方便了很多.

现在可以通过以下方式引入`Gradle`源码:

```
android {
    sourceSets {
        def base = "C:\\Users\\Administrator\\.gradle\\wrapper\\dists\\gradle-6.5-all\\2oz4ud9k3tuxjg84bbf55q0tn\\gradle-6.5\\src"
        main.java.srcDirs += [
                "${base}/core",
                "${base}/core-api",
                "${base}/base-services",
                "${base}/base-services-groovy",
                "${base}/logging",
                "${base}/plugins",
                "${base}/diagnostics",
                "${base}/ide-native",
        ]
    }

    //输出gradle版本
    println(gradle.gradleVersion)
}

```

# 环境

## 1: `com.android.tools.build:gradle:4.1.0`

### 所在的本地路径

**MAC**

> /Users/用户名/.gradle/wrapper/dists

**Win**

> C:/Users/用户名/.gradle/wrapper/dists

## 2: `gradle-6.5.zip`

### 所在的本地路径

**MAC**

```java
/Users/用户名/.gradle/caches/modules-2/files-2.1/com.android.tools.build/gradle
```

**Win**

```java
C:/Users/用户名/.gradle/caches/modules-2/files-2.1/com.android.tools.build/gradle
```

### or

**MAC**

```
‎⁨硬盘⁩ ▸ ⁨应用程序⁩ ▸ ⁨Android Studio.app⁩ ▸ ⁨Contents⁩ ▸ ⁨gradle⁩ ▸ ⁨m2repository⁩ ▸ ⁨com⁩ ▸ ⁨android⁩ ▸ ⁨tools⁩ ▸ ⁨build⁩ ▸ ⁨gradle⁩ ▸ ⁨4.1.0
```

**Win**

```
AS安装目录/⁨gradle⁩/⁨m2repository⁩/⁨com⁩/⁨android⁩/⁨tools⁩/build⁩/gradle⁩/⁨4.1.0
```

# 关键类

https://docs.gradle.org/current/javadoc/index.html?overview-summary.html

## 1. org.gradle.api.Project

`gradle/core-api`

> C:\Users\Administrator\.gradle\wrapper\dists\gradle-6.5-all\2oz4ud9k3tuxjg84bbf55q0tn\gradle-6.5\

```
println(this)
println(this.class)
println(this.name)
println(this.parent)
println(this.rootProject)
```

`gradle`文件, 被包含在一个大的`Project`对象中, 所以可以直接使用`Project`的对应方法, 获取对应的数据.

https://docs.gradle.org/current/javadoc/org/gradle/api/Project.html

## 2. com.android.build.gradle.internal.dsl.BaseAppModuleExtension

`gradle-4.1.0.jar`

> C:\Users\Administrator\.gradle\caches\modules-2\files-2.1\com.android.tools.build\gradle\4.1.0\

```
android {
    println(it.class)
}
```

`android`dsl被包含在一个`BaseAppModuleExtension`对象中.此对象中有一个成员变量:

`com.android.build.gradle.AbstractAppExtension`

```
val applicationVariants: DomainObjectSet<ApplicationVariant> = dslServices.domainObjectSet(ApplicationVariant::class.java)
```

老朋友了, 修改`apk`输出的路径和文件名, 都靠它.

## 3. com.android.build.gradle.api.ApplicationVariant

实现对象: `com.android.build.gradle.internal.api.ApplicationVariantImpl`

`gradle-4.1.0.jar`

```
applicationVariants.all { variant ->
     println(variant.class)
}
```

### 输出路径修改:

```
com.android.build.gradle.api.ApkVariant.getPackageApplicationProvider

//TaskProvider<PackageAndroidArtifact> getPackageApplicationProvider();
```

```
variant.packageApplicationProvider.get().outputDirectory = apkFolder
```

### 输出文件修改:

```
com.android.build.gradle.api.BaseVariant.getOutputs

//DomainObjectCollection<BaseVariantOutput> getOutputs();
//com.android.build.gradle.internal.api.ApkVariantOutputImpl
```

```
com.android.build.gradle.internal.api.BaseVariantOutputImpl.getOutputFileName
```

```
variant.outputs.forEach {
    it.outputFileName = "test.apk"
}
```

### 完整代码:

```
android {
    def apkFolder = new File(project.rootDir.absolutePath + "/apk")
    apkFolder.mkdirs()

    applicationVariants.all { variant ->
        if (variant.buildType.name != "debug") {
            variant.packageApplicationProvider.get().outputDirectory = apkFolder
        }
        variant.outputs.forEach {
            it.outputFileName = "test.apk"
        }
    }
}
```


# 其他版本

[Android_Gradle_DSL_4.1.0](https://github.com/angcyo/Android_Gradle_DSL_4.1.0)

[Android_Gradle_DSL_4.0.1](https://github.com/angcyo/Android_Gradle_DSL_4.0.1)

[Android_Gradle_DSL_4.0](https://github.com/angcyo/Android_Gradle_DSL_4.0)

[Android_Gradle_DSL_3.5](https://github.com/angcyo/Android_Gradle_DSL_3.5)

[Android_Gradle_DSL_3.3](https://github.com/angcyo/Android_Gradle_DSL_3.3)

[Android_Gradle_DSL_3.2](https://github.com/angcyo/Android_Gradle_DSL_3.2)

