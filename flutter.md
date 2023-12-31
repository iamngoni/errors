## Error

```bash
Sandbox: rsync.samba(52049) deny(1) file-write-create Flutter
```

### Solution

```
update your Xcode project build option 'ENABLE_USER_SCRIPT_SANDBOXING' to 'No'.
```

---

## Error

Related to `flutter_secure_storage` plugin when trying to read the encrypted data

```bash
PlatformException(Exception encountered, read, javax.crypto.BadPaddingException: error:1e000065:Cipher functions:OPENSSL_internal:BAD_DECRYPT
	at com.android.org.conscrypt.NativeCrypto.EVP_CipherFinal_ex(Native Method)
	at com.android.org.conscrypt.OpenSSLEvpCipher.doFinalInternal(OpenSSLEvpCipher.java:152)
	at com.android.org.conscrypt.OpenSSLCipher.engineDoFinal(OpenSSLCipher.java:374)
	at javax.crypto.Cipher.doFinal(Cipher.java:2056)
	at e0.h.b(StorageCipher18Implementation.java:36)
	at d0.a.c(FlutterSecureStorage.java:12)
	at d0.a.l(FlutterSecureStorage.java:18)
	at d0.e$b.run(FlutterSecureStoragePlugin.java:244)
	at android.os.Handler.handleCallback(Handler.java:938)
	at android.os.Handler.dispatchMessage(Handler.java:99)
	at android.os.Looper.loopOnce(Looper.java:210)
	at android.os.Looper.loop(Looper.java:299)
	at android.os.HandlerThread.run(HandlerThread.java:67)
, null)
```

### Solution

Add `allowBackup` and `fullBackupContent` on the application and set to `false`

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.app">
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="false"
        android:fullBackupContent="false"
    >
    ...
    </application>
</manifest>
```

### Error

Related to upgrading to Xcode15

```bash
Error (Xcode): DT_TOOLCHAIN_DIR cannot be used to evaluate LIBRARY_SEARCH_PATHS, use TOOLCHAIN_DIR instead
```

### Solution

Modify your Podfile on the post_install script to use `TOOLCHAIN_DIR` instead of `DT_TOOLCHAIN_DIR`

```bash
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    target.build_configurations.each do |config|
    	xcconfig_path = config.base_configuration_reference.real_path
    	xcconfig = File.read(xcconfig_path)
    	xcconfig_mod = xcconfig.gsub(/DT_TOOLCHAIN_DIR/, "TOOLCHAIN_DIR")
    	File.open(xcconfig_path, "w") { |file| file << xcconfig_mod }
    end
  end
end
```
