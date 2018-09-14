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
* In your app module gradle file just add the dependency:
```java
   dependencies {
    compile 'com.atlpay.android'
   }
```
## Requirements
 minimum Android 4.1 (API Level 16).
# Usage
* Step-1
```java
   Intent intentCardPayment=new Intent(MainActivity.this,CardPayment.class);
                startActivity(intentCardPayment);
```
* Step-2: set your card:

        setCart('PUT_YOUR_AMOUNT','PUT_YOUR_CURRENCY','PUT_YOUR_ORDER_NUMBER','PUT_YOUR_ORDER_DESCRIPTION');
	
	
* Step-3:  Creating a Token:
```java
   ATLPay.setSecretKey('PLACE_YOUR_SECRET_KEY_HERE');
        token = new Token(mContext);
        token.create(card,'PLACE_YOUR_EMAIL_ID_HERE', new ATLPayObserver() {
                    @Override
                    public void onRequestSuccess() {
		    // Everything went well     
                        initOrder();
                    }
                    @Override
                    public void onRequestFailure(ATLPayError atlPayError) {
			//Error Happened
                        Constants.displayToast(mContext, atlPayError.message, true);
                      
                    }
                }
        );	
    }
```
* Step-4:  Creating a Order:
 ```java
   private void initOrder() {
        String tokenId = token.getId();
        order = new Order(CardPayment.this);
        order.setTokenId(tokenId);
        order.setAmount(amount);
        order.setCurrency(currency);
        order.setDescription(orderDescription);
        order.setOrderNumber(orderNumber);
        try {
            if (null != token.getRedirectStatus() && !token.getRedirectStatus().equals("NOT_AVAILABLE")) {
                URL url = new URL("http://com.infinit.com/3dReturn");
                order.setReturnUrl(url);
            }
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        order.create(new ATLPayObserver() {
            @Override
            public void onRequestSuccess() {
	     // Everything went well  
                processOrder();
            }
            @Override
            public void onRequestFailure(ATLPayError atlPayError) {
	    //Error Happened
                Constants.displayToast(mContext, atlPayError.message, true);
            }
        });
    }
```

 * Step-5: Capture Order:
 ```java
 public void processOrder() {
        if (token.getRedirectStatus().equals("NOT_AVAILABLE")) {
            order.capture(order.getId(), new ATLPayObserver() {
                @Override
                public void onRequestSuccess() {
                    //Call Server to Consolidate Amount
                }
                @Override
                public void onRequestFailure(ATLPayError atlPayError) {
                    if (null != atlPayError.errorCode) {
                        if (atlPayError.errorCode == "INVALID_CVC") {
                            Constants.displayToast(mContext, atlPayError.message, true);
                        } else if (atlPayError.errorCode == "INCORRECT_CVC") {
                            Constants.displayToast(mContext, atlPayError.message, true);
                        } else if (atlPayError.errorCode == "INCORRECT_ZIPCODE") {
                            Constants.displayToast(mContext, atlPayError.message, true);
                        } else if (atlPayError.errorCode == "PROCESSING_ERROR") {
                            Constants.displayToast(mContext, atlPayError.message, true);
                        } else if (atlPayError.errorCode == "EXPIRED_CARD") {
                            Constants.displayToast(mContext, atlPayError.message, true);
                        } else if (atlPayError.errorCode == "UNKNOWN_ERROR") {
                            Constants.displayToast(mContext, atlPayError.message, true);
                        } else if (atlPayError.errorCode == "DECLINED") {
                            Constants.displayToast(mContext, "Your Card has been declined. Bank Returned : " + atlPayError.declineCode, true);
                        }
                    } else {
                        Constants.displayToast(mContext, atlPayError.message, true);
                    }
                    Intent mIntent = new Intent(mContext, CardPayment.class);
                    startActivity(mIntent);
                    finish();
                }
            });
        } else {
            Intent intent = new Intent(CardPayment.this, SecurePayment.class);
            intent.putExtra("frameUrl", order.getAuthUrl());
            startActivity(intent);
            finish();
        }
    }
```

