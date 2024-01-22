# flutter_github_actions

Demonstrates how to setup a simple CI/CD pipeline with [Github Actions](https://github.com/features/actions).

- TODO
  - [ ] check signing style export options
  - [ ] adapt deploy step
  - [ ] add test step
  - [ ] run sucessfully

## Setup Android

### Sign

- `brew install temurin`
- [prepare signing](https://docs.flutter.dev/deployment/android)
- `keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload`

Add the following keys to `Repo > Settings > Secrets and variables > Actions`.

- ANDROID_KEYSTORE_BASE64 -> `openssl base64 < upload-keystore.jks | tr -d '\n' | pbcopy`
- ANDROID_KEY_ALIAS -> `upload`
- ANDROID_STORE_PASSWORD -> `<add_passsword_here>`

### Deploy Android

- [create an account key as .json](https://cloud.google.com/iam/docs/keys-create-delete)

###### Sample .json file

```
{
  "type": "service_account",
  "project_id": "",
  "private_key_id": "",
  "private_key": "",
  "client_email": "",
  "client_id": "",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "",
  "universe_domain": "googleapis.com"
}
```

- add the .json content as `ANDROID_SERVICE_ACCOUNT_KEY` to `Repo > Settings > Secrets and variables > Actions`

## Setup iOS

### Secrets

Add the following keys to `Repo > Settings > Secrets and variables > Actions`:

- `IOS_CERTIFICATE_BASE64`
  - open Keychanin-Access, right click the certificate and hit export
  - run `base64 -i certificate.p12 -o base64.txt`
  - copy the base64 string to the action secret
- `IOS_CERTIFICATE_PASSWORD`
  - the password of the expoted certificate
- `IOS_PROVISION_PROFILE_BASE64`
  - download from [Apple](https://developer.apple.com/account/resources/profiles/)
  - run `base64 -i distribution.mobileprovision -o base64.txt`
  - copy the base64 string to the action secret
- `IOS_KEYCHAIN_PASSWORD`
  - choose a random password for your keychain on the virtual machine

### ExportOptions.plist

- Edit the values of the following key in [ExportOptions.plist](io/ExportOptions.plist)
  - `<key>teamID</key>` - enter the `the com.apple.developer.team-identifier` from mobile provisioning profile
  - `<key>com.example.flutter_github_actions</key>` - enter the `UUID` from mobile provisioning profile

### SemVer Usage
- https://gitversion.net/docs/reference/version-increments

## Sources

- https://blog.logrocket.com/flutter-ci-cd-using-github-actions/
- https://medium.com/team-rockstars-it/the-easiest-way-to-build-a-flutter-ios-app-using-github-actions-plus-a-key-takeaway-for-developers-48cf2ad7c72a
