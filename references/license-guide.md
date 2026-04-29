# License Guide

## 1. Purchase Official License

- **Purchase Channel**: Go to the "Audio & Video Terminal SDK Purchase Page" and select an SDK package according to your needs (monthly billing supported)
- **Permissions**: After purchase, you obtain the official License usage authorization, which is managed in the Tencent Cloud RT-Cube Console
- **Activation Mechanism**: After new purchase or renewal, the terminal app will automatically update when connected to the network. If it does not take effect, it is recommended to restart the app to clear the local cache

---

## 2. Bind Official License

> **Important**: Once the official License is submitted, information such as the package name and Bundle ID **cannot be modified**. If changes are needed, you must purchase a new resource package and create a new application to bind a new License.

### Method 1: Create a New Official Application and Select License

1. Go to Console > Mobile License, click "Create Official License"
2. Fill in **App Name**, **Package Name**, **Bundle ID**
3. Select "Effect Player License" and proceed to the next step
4. Select an unbound package to bind
5. After successful creation, the system generates **License URL** and **License Key**

### Method 2: Unlock Features in an Existing Official Application

1. Select the target application and click "Unlock New Feature"
2. Select "Effect Player License" and submit
3. Bind the package and submit for manual review

---

## 3. License Configuration on Each Platform

### Android

```java
TCMediaXBase.getInstance().setLicense(context, sLicenseUrl, sLicenseKey,
    new TCMediaXBase.ILicenseCallback() {
        @Override
        public void onResult(int errCode, String msg) {
            // errCode == 0 indicates success
        }
    });
```

### iOS

```objective-c
[[TCMediaXBase getInstance] setLicenceURL:LICENCE_URL key:LICENCE_KEY];
[[TCMediaXBase getInstance] setDelegate:self];
```

### Flutter

```dart
FTCMediaXBase.instance.setLicense(LICENSE_URL, LICENSE_KEY, (errCode, msg) {
    // errCode == 0 indicates success
});
```

---

## 4. Update Official License Validity (Renewal)

### Reminder Mechanism
The system sends reminders 32 days, 7 days, 3 days, and 1 day before expiration (supports in-app messages / email / SMS subscriptions).

### Update Methods

#### Renew Current License
- **Within validity period or expired no more than 7 days**: After renewal, validity period = original validity period + renewal duration
- **Expired more than 7 days**: The original resource is destroyed, a new resource is purchased to replace it, and validity period = payment date + renewal duration

#### Select Another License Resource to Replace
- Bind a new unbound package to replace the current resource
- Resources with auto-renewal enabled **do not support** this method

---

## 5. Auto-Renewal

### Rule
When enabled, automatic monthly renewal deduction will occur 3 days before expiration.

### Management Entry
1. **Console Management**: Log in to the RT-Cube Console > Mobile License Management page to enable/disable auto-renewal
2. **Billing Center Management**: Go to the Renewal Management page, search for "Effect Player" resources, and configure

---

## 6. License Error Code Reference

| Error Code | Description |
|-----------|-------------|
| 0 | Success |
| -1 | Invalid input parameter |
| -3 | Download failed, please check the network |
| -4, -5 | IO failure, reading local authorization info is empty |
| -6 ~ -10 | File format, signature verification, decryption failure, or JSON field error |
| -13 | Authentication failed (iOS) |
| 3015 | Bundle Id / Package Name mismatch |
| 3018 | Authorization file has expired |

### Troubleshooting Steps
1. Check whether `setLicense` was called with correct URL/KEY
2. Confirm packageName/bundleId matches the one used during application
3. Check whether the License has expired
4. Check network connection (on iOS, set after network permission is granted)
5. Check the specific error code in the callback
