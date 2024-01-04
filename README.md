# flutter_github_actions

Demonstrates how to setup a simple CI/CD pipeline with [Github Actions](https://github.com/features/actions).

## Setup Android

### Sign

* `brew install temurin`
* [prepare signing](https://docs.flutter.dev/deployment/android)
* `keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload`

Add the following keys to `Repo > Settings > Secrets and variables > Actions`.

* ANDROID_KEYSTORE_BASE64 -> `openssl base64 < upload-keystore.jks | tr -d '\n' | pbcopy`
* ANDROID_KEY_ALIAS -> `upload`
* ANDROID_STORE_PASSWORD -> `<add_passsword_here>`

### Deploy Android
* [create an account key as .json](https://cloud.google.com/iam/docs/keys-create-delete)
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
* add the .json content as `ANDROID_SERVICE_ACCOUNT_KEY` to `Repo > Settings > Secrets and variables > Actions`

## Sources
* https://blog.logrocket.com/flutter-ci-cd-using-github-actions/