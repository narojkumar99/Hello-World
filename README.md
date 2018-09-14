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
## Requirements
 minimum Android 4.1 (API Level 16).
# Usage
* Start the payment activity
```java
   Intent i = new Intent(this,PaymentActivity.class);
 startActivity(i);
```
```java
 ATLPay.setSecretKey('PLACE_YOUR_SECRET_KEY_HERE');
        token = new Token(mContext);
        token.create(card,ipAddress,uuid,'PLACE_YOUR_EMAIL_ID_HERE', new ATLPayObserver() {
                    @Override
                    public void onRequestSuccess() {
		    // Everything went well
                        mProgressDialog.dismiss();
                        initOrder();
                    }
                    @Override
                    public void onRequestFailure(ATLPayError atlPayError) {
                        mProgressDialog.dismiss();
			//Error Happened
                        Constants.displayToast(mContext, atlPayError.message, true);
                        resetBtn();
                    }
                }
        );

