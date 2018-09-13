# ATLPay Android SDK

The ATLPay Android SDK makes it easy to add atlpay payments to mobile apps.

# How to use
* In your root gradle file do the following :
```java
   allprojects {
     repositories {
	...
	maven { url 'https://jitpack.io' }
	}
   }
```
* In your app module gradle file just add the dependency
```java
   dependencies {
    compile 'com.atlpay.android'
   }
```
requires at minimum Android 4.1 (API Level 16).
# Usage
* Start the payment activity
```java
   Intent i = new Intent(this,PaymentActivity.class);
 startActivity(i);
```

