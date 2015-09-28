##在project工程文件下的build.gradle文件中

```xml
buildscript {
    repositories {
        jcenter ()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.3.0'

        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.4+'
    }
}

allprojects {
    repositories {
        jcenter ()
    }
}

```

##在app文件下的build.gradle文件中

```xml
apply plugin: 'com.android.application'
apply plugin: 'android-apt'
def AAVersion='3.2+'


android {
    compileSdkVersion 22
    buildToolsVersion "22.0.1"

    defaultConfig {
        applicationId "com.study.radasm.annotationdemo"
        minSdkVersion 17
        targetSdkVersion 22
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.2.0'
    apt "org.androidannotations:androidannotations:$AAVersion"
    compile "org.androidannotations:androidannotations-api:$AAVersion"
}

apt {
    arguments {
        androidManifestFile variant.outputs[0].processResources.manifestFile
        // if you have multiple outputs (when using splits), you may want to have other index than 0

        // You can set optional annotation processing options here, like these commented options:
        // logLevel 'INFO'
        // logFile '/var/log/aa.log'

    }
}

```

出现问题请`科学上网`！


