cordova-android-accountmanager
==============================

Introduction
------------

cordova-android-accountmanager is an [Android](https://github.com/android) [AccountManager](http://developer.android.com/reference/android/accounts/AccountManager.html) plugin for [cordova-android](https://github.com/apache/cordova-android).

It currently only supports explicit account handling (programatically adding/removing/settin/getting account details to the AccountManager).


Basic Usage (Explicit account handling)
-------

	var am = window.plugins.accountmanager;

	// Add account explicitly
	am.addAccountExplicitly('MyAccountType', 'bob', 'passwordbob', null, function(error, bob)
	{
		// bob.name = 'bob'
		// bob.type = 'MyAccountType'

		// List all accounts of MyAccountType
		am.getAccountsByType('MyAccountType', function(error, accounts)
		{
			accounts.forEach(function(account)
			{
				console.log('Account: ' + JSON.stringify(account));
			});
		});

		// Get password
		am.getPassword(bob, function(error, password)
		{
			console.log("Bob's password: " + password);
		});

		// Get/Set user data
		am.setUserData(bob, 'age', 30);
		am.getUserData(bob, 'age', function(error, age)
		{
			console.log('Bob is ' + age + 'years old');
		});

		// Remove account
		am.removeAccount(bob);
	});

	****************************************************************************
		New method added, setUserDataInbBatchMode and getUserDataInBacthMode

  ac.setUserDataInBatchMode(pushClientAccount, userData, function (data) {
    // set an object in the account manager, using the object's key-value attribute  {user:"abc", whatever:"bla",pw:"123"}
  });


  am.getUserDataInBatchMode(account, DICTIONARY, function (error, restoredValue) {
        // DICTIONARY , should contain de keys that are retrieved from de account manager
        // {user:"", whatever:"",pw:""}
  }

  *************************************** Also added:

  /hook/

Basic Usage (Authenticator based handling)
-----

	Not yet supported.


Project Setup
-------------

### Installation - Cordova Command Line

No Need to Copy or edit any file manually. Just Run Following Command and you are ready to use Account Manager (See Basic Usage above).

	$ cordova plugin add https://github.com/vishalkardode/cordova-android-accountmanager.git

### Installation - Manual ( Step 1 )

1. Copy (or link) all the .java files into your project src (under the appropriate com.polychrom.cordova package).
2. Copy accountmanager.js into your cordova app's www directory
3. Add `<script charset="utf-8" src="accountmanager.js"></script>` to your cordova app's HTML.
4. Copy (or link) authenticator.xml to your project's res/xml/ folder.

### Installation - Manual ( Step 2 ) - Edit Android Manifest for Authenticator Service

To register the AuthenticatorService with your application, the following will need to be added to your manifest's application element:

	<!-- The authenticator service -->
	<service android:name="com.polychrom.cordova.AuthenticatorService" android:exported="false">
		<intent-filter>
			<action android:name="android.accounts.AccountAuthenticator" />
		</intent-filter>
		<meta-data android:name="android.accounts.AccountAuthenticator" android:resource="@xml/authenticator" />
	</service>

### Installation - Manual ( Step 3 ) - Add Permissions

Depending on the level of usage, the following permissions may be required by the plugin for correct usage:

 * [android.permission.MANAGE_ACCOUNTS](http://developer.android.com/reference/android/Manifest.permission.html#MANAGE_ACCOUNTS)
 * [android.permission.AUTHENTICATE_ACCOUNTS](http://developer.android.com/reference/android/Manifest.permission.html#AUTHENTICATE_ACCOUNTS)
 * [android.permission.GET_ACCOUNTS](http://developer.android.com/reference/android/Manifest.permission.html#GET_ACCOUNTS)
 * [android.permission.USE_CREDENTIALS](http://developer.android.com/reference/android/Manifest.permission.html#USE_CREDENTIALS)

See the [AccountManager documentation](http://developer.android.com/reference/android/accounts/AccountManager.html) for specific requirements for each function.
