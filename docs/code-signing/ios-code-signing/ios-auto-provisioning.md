## iOS auto provisioning for Xcode projects

You can use iOS automatic provisioning to automatically generate the required Provisioning Profiles for your project.

Once you have the `.p12` and Provisioning Profiles collected by `codesigndoc`,
upload the .p12 files to your app on [bitrise.io](https://www.bitrise.io) and use the `iOS Auto Provisioning` step to manage Provisioning Profiles. `iOS Auto Provision` will generate the desired profiles and `Xcode Archive & Export for iOS` will search for these Provisioning Profiles. The iOS Auto Provision step will include the uploaded certificates in the managed profiles: the best option is to upload one Development and the Distribution Codesign Identity from the team you want to use to manage your profiles.

Before setting up automatic provisioning in your workflow, make sure that:

* you have at least __Admin__ role in the developer portal team.
* your Apple Developer account is connected to bitrise.io.
* Apple Developer Portal integration to your Bitrise project is enabled.

!!! note "Xcode Automatically manage signing option"
    The `iOS Auto Provisioning` step can automatically manage profiles even if the iOS project uses Xcode's
    _Automatically manage signing_ option, introduced in Xcode 8. The step can detect if the provided iOS
    project uses _Automatically manage signing_ option or not. If the project uses the new signing option
    the step makes sure that the test devices registered on Bitrise are also registered on the Developer
    Portal. Then it will download the Xcode managed profiles which are needed to sign your project and
    will install them together with the provided certificates.

1. Open your app on your `Dashboard`.

2. Select the `Workflow Editor` tab.

3. Select the `Code Signing` tab.

4. Add the .p12 files in the `Add the private key (.p12) for signing` field.

5. Make sure you have the `iOS Auto Provisioning` step in your app's Workflow. You can check it on the `Workflow` tab of the `Workflow Editor`.

6. Fill the required inputs of the step.
  * `The Developer Portal team id` - find this on the [Membership Details page of your Apple Developer Portal account](https://developer.apple.com/account/#/membership)
  * `Distribution type` - make sure its value matches the value of the `Select method for export` input in the `Xcode Archive & Export for iOS step`.
  * `Scheme` - you can restrict which targets to process.

6. Make sure that you do __NOT__ have the `Certificate and profile installer` step in your Workflow. If you have both `iOS Auto Provisioning` and `Certificate and profile installer` steps in your Workflow, your build will fail.


!!! note "Troubleshooting: missing Distribution signing files"
    If `codesigndoc` does not pick up one or more distribution .p12 files,
    you can export those manually from the `Keychain Access` app, just like you would when you
    transfer these files between Macs.

    But __even if `codesigndoc` does not find
    all the files, you should upload all the files collected by `codesigndoc` - except the Provisioning Profile files!__
    The base files collected by `codesigndoc` are essential for your project's
    code signing: without those it's not possible to create a signed IPA
    for the project!
