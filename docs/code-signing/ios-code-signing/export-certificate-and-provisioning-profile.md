## Exporting and uploading iOS code signing identities

The Provisioning Profile(s) and Code Signing Identity (.p12 Certificate) are
crucial part of the development process for iOS and Mac applications.
The Provisioning Profile contains application related data,
the list of devices that can run the given application, the connected Certificates and many more.

The Code Signing Identity (.p12 Certificate) contains information about the developer
and makes it possible to sign the application. Both of these files are needed to build your application,
test them on devices or upload them to the AppStore.

You can easily locate the needed certificates and provisioning profiles for your iOS project with our `codesigndoc` tool.

### Exporting code signing files using the codesigndoc tool

1. Open your `Terminal.app` on your Mac.
[run the one liner "install" command](https://github.com/bitrise-tools/codesigndoc#one-liner).

1. Enter the appropriate one-liner command, depending on your project type.

    * For an __Xcode__ project:

      `bash -l -c "$(curl -sfL https://raw.githubusercontent.com/bitrise-tools/codesigndoc/master/_scripts/install_wrap-xcode.sh)"
`

    * For a __Xamarin__ project:

      `bash -l -c "$(curl -sfL https://raw.githubusercontent.com/bitrise-tools/codesigndoc/master/_scripts/install_wrap-xamarin.sh)"`

1. Open your `Finder.app` and drag-and-drop your project's `.xcodeproj` or `.xcworkspace` file into the command line in your `Terminal`.

You now have all the required files exported, ready for upload!

### Uploading the exported code signing files to Bitrise

Once you have collected all the needed files with `codesigndoc`, what's left to be done is uploading them to [bitrise.io](https://www.bitrise.io).

1. Head to your Dashboard on [bitrise.io](https://www.bitrise.io) and select your app.

2. Go to **Workflow** > **Manage Workflows** > and select the **Code Signing & Files** tab on the left.

3. Upload your code signing certificate (p12) and Provisioning Profiles and you are ready to go! 🚀


!!! warning "Make sure you can archive and export on your Mac!"
    `codesigndoc` only works if you can archive and export your app from `Xcode.app` -
    until you get a signed IPA!

`Xcode.app` might auto generate files in the background
during the export process, and obviously `codesigndoc` can only collect those files
after the files are available on your Mac.

__This means that you should first Archive the project in `Xcode.app`__,
Export it for the distribution type you want to use (`Ad Hoc`, `App Store` or `Enterprise`),
and __run `codesigndoc` after you have the `.ipa` file generated by `Xcode.app`__.
This way `codesigndoc` can collect all the code signing files
required for that type of distribution.


### Manually exporting Provisioning Profiles

1. Visit the [Apple Developer Portal](https://developer.apple.com/) - use your AppleID to login.
1. Select the [Certificates, IDs & Profiles](https://developer.apple.com/account/ios/certificate/) section.
1. Find the Provisioning Profile you need,
   select it and click download (the file extension is `.mobileprovision`
   in case of an iOS Provisioning Profile, and `.provisionprofile` in case of a macOS application Provisioning Profile)
1. To upload it to your app on [bitrise.io](https://www.bitrise.io)
    * open your app on [bitrise.io](https://www.bitrise.io)
    * select the `Workflow` tab
    * in the Workflow Editor, on the left side, select the `Code signing & Files` option
    * here you can upload your Provisioning Profiles and your Code Signing Identities (`.p12` Certificate)

### Manually exporting and uploading the Certificate (.p12 Identity)

#### To request/create a signing certificate

1. Request a Certificate from `Xcode.app`'s `Accounts` section in `Preferences`,
   or from the [Apple Developer Portal](https://developer.apple.com/) manually.

#### Download signing certificate from the Apple Developer Portal

1. Visit the [Certificates, IDs & Profiles](https://developer.apple.com/account/ios/certificate/) section
   of the Apple Developer Portal.
1. Choose `Certificates` on the left side
1. Select the Certificate and click download (the file extension is `.cer`)
1. Open the file once the download is finished
1. This will open the certificate in your `Keychain Access.app`

#### Export the certificate (.p12 identity)

1. Open `Keychain Access.app`
1. On the left side, select `My Certificates`
1. Right click on the certificate you want to exported
1. Select "Export .." in the menu

_Note: you can select more than one certificate at the same time, then right
click and select "Export ..." - this will export all the certificates
into a __single `.p12` file__!_

To upload the .p12 signing certificate file to your app on [bitrise.io](https://www.bitrise.io):

* open your app on [bitrise.io](https://www.bitrise.io)
* select the `Workflow` tab
* in the Workflow Editor, on the left side, select the `Code signing & Files` option
* here you can upload your Provisioning Profiles and your Code Signing Identities (`.p12` Certificate)

!!! note "More information about how iOS code signing works"
    For more information about how iOS code signing works, please
    check the [iOS/Code Signing](/ios/code-signing) page.
