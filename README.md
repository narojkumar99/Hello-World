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
* Step-2: set your card
```java
        setCart('PUT_YOUR_AMOUNT','PUT_YOUR_CURRENCY','PUT_YOUR_ORDER_NUMBER','PUT_YOUR_ORDER_DESCRIPTION');
	
	```
* Step-3:
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
                        resetBtn();
                    }
                }
        );	
    }
```

* Step-4:

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
                processOrder();
            }
            @Override
            public void onRequestFailure(ATLPayError atlPayError) {
                Constants.displayToast(mContext, atlPayError.message, true);
            }
        });
    }
```

 * Step-5:


