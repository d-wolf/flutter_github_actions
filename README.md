# flutter_github_actions

A new Flutter project.

## Sign Android

* `brew install temurin`
* [prepare signing](https://docs.flutter.dev/deployment/android)
* `keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload`

* Add the following to GitHub repository > Secrets > Actions

* ANDROID_KEYSTORE_BASE64 -> `openssl base64 < upload-keystore.jks | tr -d '\n' | pbcopy`
* ANDROID_KEY_ALIAS -> `upload`
* ANDROID_PLAYSTORE_ACCOUNT_KEY -> ``
* ANDROID_STORE_PASSWORD -> `<your-pw>`

## Sources
* https://blog.logrocket.com/flutter-ci-cd-using-github-actions/