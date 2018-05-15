### iOS manual provisioning

Once you have the `.p12` and Provisioning Profiles collected by `codesigndoc`,
upload the files to your app on [bitrise.io](https://www.bitrise.io).

1. Open your app on your `Dashboard`.

2. Select the `Workflow Editor` tab.

3. Select the `Code Signing` tab.

4. Add the Provisioning Profile files and the .p12 files in the `Add Provisioning Profile(s)` and the `Add the private key (.p12) for signing` fields, respectively.

5. Make sure you have the `Certificate and profile installer` step in your app's Workflow. You can check it on the `Workflow` tab of the `Workflow Editor`.


!!! note "Troubleshooting: missing Distribution signing files"
    If `codesigndoc` does not pick up one or more distribution .p12 files and/or Provisioning Profile(s),
    you can export those manually (.p12 from `Keychain Access` app, Provisioning Profiles from
    [Apple Developer Portal](https://developer.apple.com/)), just like you would when you
    transfer these files between Macs.

    But __even if `codesigndoc` does not find
    all the files, you should upload all the files collected by `codesigndoc`!__
    The base files collected by `codesigndoc` are essential for your project's
    code signing: without those it's not possible to create a signed IPA
    for the project!
