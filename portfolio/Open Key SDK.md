<h1>Open Key Android SDK</h1>

[TOC]
<h2> Overview</h2>
This SDK allows you to implement a Hotel guest Android app that can communicate mobile keys in order to open door locks.

The implementation uses Android Beam to enable communication of the room `key` via NFC (Near Field Communication) between the Android app and the door lock.  A callback interface is established between the app and the Android device, and an NDEF message is passed to Android Beam to open the door lock.

A successful implementation of the SDK includes the following:

1. Sets up NFC Communication
2.  Establishes an NDEF Message Callback interface
4. Optionally establishes the NDEF Push Complete interface, to inform the app that the NDEF push message completed successfully.

<h2>Installation</h2>

The Open Key Android SDK is distributed as two JAR libraries.  To get started, simply copy the the files (Guest.jar and GuestHelper.jar) into the libs folder of your project directory.

<h2>Declaring Permissions</h2>

Your app will use the NFC hardware on the Android device in order to invoke the callback interface using NDEF push (Android Beam).  

You use the `<uses-permission` element in the AndroidManifest.xml file to enable access to the NFC hardware:

```java
<uses-permission android:name="android.permission.NFC" />
```

You must also declare read only access to the phone's state:

``` java
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

<h2>Setting Up NFC</h2>

In the Main Activity of your application you can instantiate a new object of the [NfcAdapter](http://developer.android.com/reference/android/nfc/NfcAdapter.html) class called `mNfcAdapter.`  

``` java
--- private NfcAdapter mNfcAdapter;
```

Once `mNfcAdapter` is declared, you can check that the Android device has compatible NFC hardware by calling the following statement:

``` java
--- mNfcAdapter = NfcAdapter.getDefaultAdapter(MainActivity.this);

```
If there is a compatibility issue with the device's NFC hardware, you should inform the user, via an appropriate error message:

```java
if (mNfcAdapter != null) 
{	
	
}else 
{
// appropriate error message
```


<h2>NDEF Message Callback Interface</h2>

In order to allow the Hotel Guest app to open the lock, you create a callback interface that is an implementation of [NfcAdapter.CreateNdefMessageCallback](http://developer.android.com/reference/android/nfc/NfcAdapter.CreateNdefMessageCallback.html).   This implementation overrides the super class method [createNdefMessage](http://developer.android.com/reference/android/nfc/NfcAdapter.CreateNdefMessageCallback.html#createNdefMessage%28android.nfc.NfcEvent%29).

In order to sucesfully establish this interface

1. Instantiate the `Manager` class using the object `lockManager`
2. Call the `createNdf` method of `Manager`, in order to pass the NDEF message that will open the lock via Android Beam.

<h3>Instantiating the Manager Class</h3>

You need to create an instance of the Manager class in order to establish NFC communications with the lock.  When you create the object you must assign the `mNfcAdapter` as a parameter.  You must also include the `MainActivity` of the application and the Ndef message callback interface as parameters to this object.  This is illustrated in the code example below:

```java
}
		lockManager = new Manager(mNfcAdapter, MainActivity.this, this);
	}
```

<h3>Using the createNdef Method</h3>

The `createNdef` method of the `Manager` class enables you to pass the Guest mobile `key` from Lock Mobile Key Gen to the lock.  When you call this method, it will return the appropriate NDEF message and guest mobile key, properly formatted for delivery to the lock via Android Beam.  This is illustrated below

```java

@Override
	public NdefMessage createNdefMessage(NfcEvent event) {
		// TODO Auto-generated method stub
		return lockManager.createNdef(key);
```

<h2>NDEF Push Complete Interface</h2>

You can enable a [NfcAdapter.OnNdefPushCompleteCallback](http://developer.android.com/reference/android/nfc/NfcAdapter.OnNdefPushCompleteCallback.html), in order to have the Android device inform your app when the NDEF Push is completed by Android Beam.  You will need to override the method as follows

```java
@Override
 public void onNdefPushComplete(NfcEvent event) 
{
// TODO Auto-generated method stub
}
```
Once you've implemented the interface, provide the callback to the adapter

```java
mNfcAdapter.setOnNdefPushCompleteCallback(this, MainActivity.this);
```


----------


---






> Written with [StackEdit](https://stackedit.io/).