## iOS Code Signing

The code signing of iOS and Mac projects requires:

* `.p12` Certificate / Identity file(s)
* __Provisioning Profile__ file(s) matching your project (team ID, bundle ID, ...)
* a script, tool or step which installs these files in the build environment.

You can store your code signing files and create a signed .ipa file for your iOS, Mac or Xamarin project on [bitrise.io](https://www.bitrise.io). You can manually upload all the required files (Provisoning Profiles and .p12 certificate files) or you can use automatic provisioning to automatically generate and manage Provisioning Profiles from a connected Apple Developer account. We'll show you how to use both options!

1. [Collect the required files with codesigndoc](#collect-the-required-files-with-codesigndoc).

1. Upload and manage your files with either [manual provisioning](#iOS-manual-provisioning) or [automatic provisioning](#iOS-auto-provisioning).

1. Use the `Xcode Archive & Export for iOS`, the `Xcode Archive for Mac`, or the `Xamarin Archive` step to create a signed `.ipa`:

    * Xcode projects: [Configure the appropriate `Xcode Archive` step to create the signed `.ipa`](#configure-the-appropriate-xcode-archive-step-to-create-the-signed-ipa)
    * Xamarin projects: [Configure `Xamarin Archive` to create the signed `.ipa`](#configure-the-xamarin-archive-step-to-create-the-signed-ipa)
