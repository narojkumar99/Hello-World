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
* Start the payment activity
 Intent i = new Intent(this,PaymentActivity.class);
 startActivity(i);

