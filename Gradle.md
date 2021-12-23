Android studio sử dụng Gradle để xây dựng, tạo và triển khai APK. </br>
## Gradle From Android's Context
Bất cứ khi nào tạo một dự án android có 3 file gradle mặc định: settings.gradle, build.gradle, app/build.gradle </br>
* settings.gradle: tất cả các module cần thiết cho đự án được xác định ở đây.
  ```
  include ":app", ":permission"
  ```
* build.gradle: đây là file build.gradle của dự án gốc, bất kỳ tùy chọn cấu hình nào ở đây sẽ là chung cho cả module
  ```
  // Top-level build file where you can add configuration options common to all sub-projects/modules.

  buildscript {
      repositories {
          jcenter()
          google()
      }
      dependencies {
          classpath 'com.android.tools.build:gradle:3.0.1'

          // NOTE: Do not place your application dependencies here; they belong
          // in the individual module build.gradle files
      }
  }

  allprojects {
      repositories {
          jcenter()
          google()
      }
  }

  task clean(type: Delete) {
      delete rootProject.buildDir
  }
  ```
  * Khối lệnh trong từ khóa `buildscript` thể hiện nơi mà Gradle có thể download plugin trên
  * Gradle cho phép chúng ta tạo các task của riêng chúng ta. Mặc định ở trên, khi khởi tạo project, một task clean đã được thêm vào build.gradle. Dòng type: Delete chỉ ra rằng task clean là một loạt task Delete của Gradle. Trong trường hợp trên, clean task sẽ xóa bỏ thư mục build từ thư mục root của project.
* app/build.gradle: 
  ```
  apply plugin: 'com.android.application'

  android {
      compileSdkVersion 26
      buildToolsVersion "26.0.1"
      defaultConfig {
          applicationId "com.flatpro"
          minSdkVersion 15
          targetSdkVersion 26
          versionCode 2
          versionName "2.0"
          testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
      }
      buildTypes {
          release {
              minifyEnabled false
              proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
          }
      }
      repositories {
          jcenter()
          maven {
              url "https://jitpack.io"
          }
      }
      testOptions {
          unitTests.returnDefaultValues = true
      }
  }

  dependencies {
      compile fileTree(include: ['*.jar'], dir: 'libs')
  //    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
  //        exclude group: 'com.android.support', module: 'support-annotations'
  //    })
      compile 'com.github.darsh2:MultipleImageSelect:v0.0.4'
      compile 'me.dm7.barcodescanner:zxing:1.9.8'
      compile 'com.squareup.okhttp3:okhttp:3.9.1'
      compile 'com.squareup.retrofit2:retrofit:2.5.0'
      compile 'com.squareup.retrofit2:converter-gson:2.5.0'
      compile 'com.squareup.okhttp3:logging-interceptor:3.4.1'
      compile files('libs/mint-5.2.2.jar')
      testCompile "junit:junit:4.12"
      compile "com.android.support:support-v4:26.1.0"
      compile project(':base')
  }
  ```
  * File app/build.gradle được sử dụng cho riêng module app, mỗi module sẽ có một file build.gradle riêng. Dòng đầu tiên thể hiện đây là một Android Application module (module chính để chạy project Android)
  * apply plugin: "com.android.library"Khối lệnh nằm trong android thể hiện các cấu hình cho module app
  * khối lệnh nằm trong dependencies, đây là nơi khai báo các thư viện, các module mà chúng ta sẽ sử dụng trong project
  * fileTree dependency có nghĩa là tất cả các file .jar nằm trong thư mục libs sẽ được thêm vào compile classpath.androidTestCompile và testCompile thể hiện các thư viện được sử dụng để test trong project.compile thể hiện các module, các thư viện (kèm các version) mà chúng ta sẽ sử dụng trong project.
