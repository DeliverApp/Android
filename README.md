# Using DeliverApp Android SDK via GitHub Packages

This guide explains how to add our Android library, hosted on GitHub Packages, to your Android project.

## Step 1: Add the GitHub Packages Repository to `settings.gradle.kts`

To access the DeliverApp library from GitHub Packages, add the Maven repository to your `settings.gradle.kts` file:

```kotlin
// settings.gradle.kts

pluginManagement {
    repositories {
        maven {
            url = uri("https://maven.pkg.github.com/DeliverApp/Android")
            credentials {
                username =System.getenv("GITHUB_USERNAME") ?: ""
                password = System.getenv("GITHUB_TOKEN") ?: ""
            }
        }
        gradlePluginPortal()
        mavenCentral()
    }
}
```

## Step 2: Use Environment Variable for GitHub Username and Token

GitHub Packages use Personal access tokens to download package unless the repository is public.
If you already has a token with **read:packages** privilege, you can use it.
If not, you could set a new token by visit https://github.com/settings/tokens.
Finally, if you really don't want to create your own token, Deliver offers you a token you can use.
Be careful, it expires every 90 days.

### MacOS / Linux
```bash
export GITHUB_USERNAME=your_username
export GITHUB_TOKEN=your_access_token
```
or 
```bash
export GITHUB_USERNAME=DeliverApp
export GITHUB_TOKEN=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 // expire every 90 days, ask for faster regeneration
```

### Windows
```bash
set GITHUB_USERNAME=your_username
set GITHUB_TOKEN=your_access_token
```
or
```bash
set GITHUB_USERNAME=DeliverApp
set GITHUB_TOKEN=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 // expire every 90 days, ask for faster regeneration
```

## Step 3: Add the Library Dependency

```kotlin
// build.gradle.kts

dependencies {
    implementation("deliver.io:sdk:latest")
}
```

## Step 4: How to use it

Deliver Android SDK provide two initialize methods you could use in your Android Application.
First, Find your apiKey on https://deliverapp.io/store/app/:appId/details (TODO).

### Init without OkHttpClient.Builder

You can just init the SDK with your app apiKey.

```kotlin
// Application.kt

DeliverSDK.init(this, "APP_API_KEY")
```

If youd do so this way, you will have access to all the marvelous features SDK offers except Network remote debugging.
To unlock this one, you have to use our okhttp3 interceptor.

```kotlin
your_okhttp3_builder.addNetworkInterceptor(DeliverHTTPInterceptor()).build()
```

### Init with OkHttpClient.Builder

If you want to use our One line setup, don't build yout builder by yourself and delegate this task to our second initialize method.

```kotlin
// Application.kt

your_okhttp3_client = DeliverSDK.init(this, "APP_API_KEY", your_okhttp3_builder)
```

### And that's it ! Start using magic

Thank you for choosing Deliver !
