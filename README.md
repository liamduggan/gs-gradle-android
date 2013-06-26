# Getting Started: Building Android Projects with Gradle

What you'll build
-----------------

This guide walks you through using Gradle to build a simple Android project.

What you'll need
----------------

 - About 15 minutes
 - A favorite text editor or IDE
 - [Android SDK][sdk]
 - An Android device or Emulator

[sdk]: http://developer.android.com/sdk/index.html

How to complete this guide
--------------------------

Like all Spring's [Getting Started guides](/getting-started), you can start from scratch and complete each step, or you can bypass basic setup steps that are already familiar to you. Either way, you end up with working code.

To **start from scratch**, move on to [Set up the project](#scratch).

To **skip the basics**, do the following:

 - [Download][zip] and unzip the source repository for this guide, or clone it using [git](/understanding/git):
`git clone https://github.com/springframework-meta/{@project-name}.git`
 - cd into `{@project-name}/initial`
 - Jump ahead to [Create a resource representation class](#initial).

**When you're finished**, you can check your results against the code in `{@project-name}/complete`.

<a name="android-dev-env"></a>
Install the Android development environment
----------------------------------------------

Building Android applications requires you to install the [Android SDK][sdk]. Installing the Android SDK also installs the AVD Manager, which provides a graphical user interface for creating and managing Android Virtual Devices (AVDs). 

### Install the Android SDK

1. Download the correct version of the [Android SDK][sdk] for your operating system from the Android web site.

2. Unzip the archive to a location of your choosing. For example, on Linux or Mac, you could place it in the root of your user directory. See the [Android Developers] web site for additional installation details.

3. Configure the `ANDROID_HOME` environment variable based on the location of the Android SDK. Additionally, consider adding `ANDROID_HOME/tools`, and  `ANDROID_HOME/platform-tools` to your PATH.

    Mac OS X:

    ```sh
    $ export ANDROID_HOME=/<installation location>/android-sdk-macosx
    $ export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
    ```

    Linux:

    ```sh
    $ export ANDROID_HOME=/<installation location>/android-sdk-linux
    $ export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
    ```

    Windows:

    ```sh
    set ANDROID_HOME=C:\<installation location>\android-sdk-windows
    set PATH=%PATH%;%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools
    ```

### Install Android SDK platforms and packages

The [Android SDK][sdk] download does not include specific Android platforms. To run the code in this guide, you need to download and install the latest SDK Platform. You do this by using the Android SDK and AVD Manager that was installed from the previous step.

1. Open the **Android SDK Manager** window:

    ```sh
    $ android
    ```

    > Note: If this command does not open the *Android SDK Manager*, then your path is not configured correctly.

2. Select the checkbox for **Tools**.

3. Select the checkbox for the latest Android SDK, Android 4.2.2 (API Level 17) as of this writing.

4. Select the checkbox for the **Android Support Library** from the **Extras** folder.

5. Click the **Install packages...** button to complete the download and installation.

    > Note: You may want to install all the available updates, but be aware it will take longer, as each API level is a large download.

[sdk]: http://developer.android.com/sdk/index.html
[Android Developers]: http://developer.android.com/sdk/installing/index.html
[Platforms and Packages]: http://developer.android.com/sdk/installing/adding-packages.html

<a name="scratch"></a>
Set up the project
------------------

First, you will need to set up an Android project for Gradle to build. To keep the focus on Gradle, make the project as simple as possible for now.

### Create the directory structure

In a project directory of your choosing, create the following subdirectory structure; for example, with the following command on Mac or Linux:

```sh
$ mkdir -p src/main/java/org/hello
```

    └── src
        └── main
            └── java
                └── org
                    └── hello

### Create an Android manifest

The [Android Manifest] contains all the information required to run an Android application, and it cannot build without one.

[Android Manifest]: http://developer.android.com/guide/topics/manifest/manifest-intro.html

`src/main/AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.hello"
    android:versionCode="1"
    android:versionName="1.0.0" >

    <application android:label="@string/app_name" >
        <activity
            android:name=".HelloActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

### Create a String Resource

`src/main/res/values/strings.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">Android Gradle</string>
</resources>
```

### Create a Layout

`src/main/res/layout/hello_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView  
    android:id="@+id/text_view"
    android:layout_width="fill_parent" 
    android:layout_height="wrap_content" 
    />
</LinearLayout>

```

### Create Java classes

Within the `src/main/java/org/hello` directory, you can create any Java classes you want. To maintain consistency with the rest of this guide, create the following class:

```java
package org.hello;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class HelloActivity extends Activity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.hello_layout);
    }

    @Override
    public void onStart() {
        super.onStart();
        TextView textView = (TextView) findViewById(R.id.text_view);
        textView.setText("Hello world!");
    }

}

```

### Install Gradle

Now that you have a project that you can build with Gradle, you can install Gradle. 

1. Download the latest version of Gradle (1.6 as of this writing) from the [Gradle Downloads] page 

    > Note: Only the binaries are required, so look for the link to `gradle-1.6-bin.zip`. Alternatively, you may choose `gradle-1.6-all.zip` to download the sources and documentation as well as the binaries.

2. Unzip the archive and place it in a location of your choosing. For example on Linux or Mac, you may want to place it in the root of your user directory. See the [Installing Gradle] page for additional details.

3. Configure the `GRADLE_HOME` environment variable based on the location where you installed Gradle.

    Mac/Linux:

    ```sh
    $ export GRADLE_HOME=/<installation location>/gradle-1.6
    $ export PATH=${PATH}:$GRADLE_HOME/bin
    ```

    Windows:

    ```sh
    set GRADLE_HOME=C:\<installation location>\gradle-1.6
    set PATH=%PATH%;%GRADLE_HOME%\bin
    ```

4. Test the Gradle installation with following command:

    ```sh
    $ gradle
    ```

    If the installation is correct, we will see a welcome message that looks something like the following:

    ```sh
    :help
    
    Welcome to Gradle 1.6.
    
    To run a build, run gradle <task> ...
    
    To see a list of available tasks, run gradle tasks
    
    To see a list of command-line options, run gradle --help
    
    BUILD SUCCESSFUL
    
    Total time: 2.923 secs 
    ```

You now have Gradle installed.


Find out what Gradle can do
------------------------------
Now that Gradle is installed, kick the tires on it and see what it can do. Before you even create a build.gradle file for the project, you can ask it what tasks are available:

```sh
$ gradle tasks
```

You should see a list of available tasks. Assuming you run Gradle in a folder that does not contain a _build.gradle_ file, you will see some basic tasks such as the following:

```sh
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'gs-gradle-android'.
dependencyInsight - Displays the insight into a specific dependency in root project 'gs-gradle-android'.
help - Displays a help message
projects - Displays the sub-projects of root project 'gs-gradle-android'.
properties - Displays the properties of root project 'gs-gradle-android'.
tasks - Displays the tasks runnable from root project 'gs-gradle-android' (some of the displayed tasks may belong to subprojects).

To see all tasks and more detail, run with --all.

BUILD SUCCESSFUL

Total time: 2.706 secs
```

Even though these tasks are available, they do not offer much value without a project build configuration, however as we complete our `build.gradle` file, some of these tasks will be more useful. The list of tasks will grow as we add plugins to our `build.gradle` file, so you may want to run `gradle tasks` again to see what new tasks are available.


Build Android code
------------------

The most simple Android project has the following `build.gradle`:

```groovy
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.4.2'
    }
}

apply plugin: 'android'

android {
    buildToolsVersion "17.0"
    compileSdkVersion 17
}
```

This build configuration brings a significant amount of power. If you run `gradle tasks` again, we see some new tasks added to the list, including tasks for building the project, creating JavaDoc, and running tests.

The `build` task is probably one of the tasks we use most often with Gradle. This task compiles, tests, and packages the code into an APK file. You can run it like this:

```sh
$ gradle build
```

After a few seconds, we see the words "BUILD SUCCESSFUL". The complete output can be seen below. 

```sh
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Android tasks
-------------
androidDependencies - Displays the Android dependencies of the project
signingReport - Displays the signing info for each variant

Build tasks
-----------
assemble - Assembles all variants of all applications and secondary packages.
assembleDebug - Assembles all Debug builds
assembleRelease - Assembles all Release builds
assembleTest - Assembles the Test build for the Debug build
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
clean - Deletes the build directory.

Build Setup tasks
-----------------
setupBuild - Initializes a new Gradle build. [incubating]

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'complete'.
dependencyInsight - Displays the insight into a specific dependency in root project 'complete'.
help - Displays a help message
projects - Displays the sub-projects of root project 'complete'.
properties - Displays the properties of root project 'complete'.
tasks - Displays the tasks runnable from root project 'complete' (some of the displayed tasks may belong to subprojects).

Install tasks
-------------
installDebug - Installs the Debug build
installTest - Installs the Test build for the Debug build
uninstallAll - Uninstall all applications.
uninstallDebug - Uninstalls the Debug build
uninstallRelease - Uninstalls the Release build
uninstallTest - Uninstalls the Test build for the Debug build

Verification tasks
------------------
check - Runs all checks.
connectedCheck - Runs all device checks on currently connected devices.
connectedInstrumentTest - Installs and runs the tests for Build 'Debug' on connected devices.
deviceCheck - Runs all device checks using Device Providers and Test Servers.

Rules
-----
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.
Pattern: clean<TaskName>: Cleans the output files of a task.

To see all tasks and more detail, run with --all.

BUILD SUCCESSFUL

Total time: 6.871 secs
```

The results of the build process can be found in the *build* folder. Here we see several folders related to various parts of the build or application. The assembled Android package can be found in the *apk* folder. The APK file found here is ready to be deployed to a device or emulator


Declare Dependencies
--------------------

The simple Hello World sample is completely self-contained and does not depend on any additional libraries. Most application, however, depend on external libraries to handle common and/or complex functionality.

For example, suppose you want the application to print the current date and time. You could use the date and time facilities in the native Java libraries, but you can make things more interesting by using the Joda Time libraries.

To do this, modify HelloActivity.java to look like this:

`src/main/java/org/hello/HelloActivity.java`
```java
package org.hello;

import org.joda.time.LocalTime;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class HelloActivity extends Activity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.hello_layout);
    }

    @Override
    public void onStart() {
        super.onStart();
        LocalTime currentTime = new LocalTime();
        TextView textView = (TextView) findViewById(R.id.text_view);
        textView.setText("The current local time is: " + currentTime);
    }

}
```

In this example, we are using Joda Time's `LocalTime` class to retrieve and display the current time. 

If you ran `gradle build` to build the project now, the build would fail because you have not declared Joda Time as a compile dependency in the build. You can fix that by adding the following lines to build.gradle:

```groovy
repositories { mavenCentral() }
dependencies {
    compile "joda-time:joda-time:2.2"
}
```

The first line here indicates that the build should resolve its dependencies from the Maven Central repository. Gradle leans heavily on many conventions and facilities established by the Maven build tool, including the option of using Maven Central as a source of library dependencies.

Within the `dependencies` block, you declare a single dependency for Joda Time. Specifically, you are asking for (reading right to left) version 2.2 of the joda-time library, in the joda-time group. 

Another thing to note about this dependency is that it is a `compile` dependency, indicating that it should be available during compile-time. Now if we run `gradle build`, Gradle should resolve the Joda Time dependency from the Maven Central repository and the build will be successful.

The completed build.gradle now looks like the following:

`build.gradle`
```gradle
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.4.2'
    }
}

apply plugin: 'android'

android {
    buildToolsVersion "17.0"
    compileSdkVersion 17
}

repositories { 
    mavenCentral() 
}

dependencies {
    compile("joda-time:joda-time:2.2")
}
```


Summary
-------
Congratulations! You have now created a simple yet effective Gradle build file for building Android projects.


[New Build System]:http://tools.android.com/tech-docs/new-build-system
[Gradle]:http://www.gradle.org
[Gradle Downloads]:http://www.gradle.org/downloads
[Installing Gradle]:http://www.gradle.org/docs/current/userguide/installation.html
[Building Android Projects with Maven]:https://github.com/springframework-meta/gs-maven-android
