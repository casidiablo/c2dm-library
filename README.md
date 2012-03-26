Cloud to Device Messaging Library
=================================

This is just a mavenized, improved version of the [C2DM][1] classes used by some demo apps. It is meant to make
it easier for any Android developer to implement [C2DM][1] in his/her app. Before you use this library, make sure
you already have [signup to C2DM][2].

###Usage

\1. Just add this to your POM:

```xml
<dependency>
    <groupId>com.codeslap</groupId>
    <artifactId>c2dm-library</artifactId>
    <version>1.0.0</version>
    <scope>compile</scope>
</dependency>
```

or [download the JAR][3] from the Maven Central Repository and link it manually to your project.

\2. Create a class that extends [`com.google.android.c2dm.C2DMBaseReceiver`][4] (remember that, since this class is
actually an Android [`Service`][5], you must declare a default constructor; and you must send the registered sender email to the super class. For
instance: `super("your_registerd_sender_id@gmail.com")`). Then modify your manifest to look like this:

```xml
<application... >

    <!-- C2DM service and receiver -->
    <service android:name="com.your.package.name.YourReceiverImplementation"/>
    <receiver android:name="com.google.android.c2dm.C2DMBroadcastReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
        <meta-data android:name="com.google.android.c2dm.implementation"
                   android:value="com.your.package.name.YourReceiverImplementation"/>
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
            <category android:name="com.your.package.name"/>
        </intent-filter>
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
            <category android:name="com.your.package.name"/>
        </intent-filter>
    </receiver>

</application>

<permission android:name="com.your.package.name.C2D_MESSAGE" android:protectionLevel="signature"/>
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"/>
<uses-permission android:name="com.your.package.name.permission.C2D_MESSAGE"/>
```

3\. When necessary, register the device using `C2DMessaging.register(String)` method. For instance:

```java
// this can be placed in your home activity.
String registrationId = C2DMessaging.getRegistrationId(this /**context**/);
if (TextUtils.isEmpty(registrationId)) {
    C2DMessaging.register(this, "your_registerd_sender_id@gmail.com");
}
```

Do not forget to send the registration ID to your server from the `onRegistration` method of your
[`com.google.android.c2dm.C2DMBaseReceiver`][4] implementation. You need that ID to send push notifications to
the clients.

  [1]: http://code.google.com/android/c2dm/index.html
  [2]: http://code.google.com/android/c2dm/signup.html
  [3]: http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.codeslap%22%20a%3A%22c2dm-library%22
  [4]: https://github.com/casidiablo/c2dm-library/blob/master/src/main/java/com/google/android/c2dm/C2DMBaseReceiver.java
  [5]: http://developer.android.com/reference/android/app/Service.html