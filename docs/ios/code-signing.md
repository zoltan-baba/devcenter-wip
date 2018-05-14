## iOS Code Signing

The code signing of iOS and Mac projects requires:

* `.p12` Certificate / Identity file(s)
* __Provisioning Profile__ file(s) matching your project (team ID, bundle ID, ...)
* a script, tool or step which installs these files in the build environment.

You can store your code signing files and create a signed .ipa file for your iOS, Mac or Xamarin project on [bitrise.io](https://www.bitrise.io). You can manually upload all the required files (Provisoning Profiles and .p12 certificate files) or you can use automatic provisioning to automatically generate and manage Provisioning Profiles from a connected Apple Developer account. We'll show how to use both options!

1. [Collect the required files with codesigndoc](#collect-the-required-files-with-codesigndoc).

1. Upload and manage your files with either [manual provisioning](#iOS-manual-provisioning) or [automatic provisioning](#iOS-auto-provisioning).

1. Use the `Xcode Archive & Export for iOS`, the `Xcode Archive for Mac`, or the `Xamarin Archive` step to create a signed `.ipa`:

    * Xcode projects: [Configure the appropriate `Xcode Archive` step to create the signed `.ipa`](#configure-the-appropriate-xcode-archive-step-to-create-the-signed-ipa)
    * Xamarin projects: [Configure `Xamarin Archive` to create the signed `.ipa`](#configure-the-xamarin-archive-step-to-create-the-signed-ipa)


### Collect the required files with codesigndoc

The open source [codesigndoc](https://github.com/bitrise-tools/codesigndoc)
tool runs a clean Xcode/Xamarin Studio Archive _on your Mac_, and analyzes the generated archive file. It collects the code signing settings that Xcode or Xamarin Studio used during the archive process, and prints the list of the required code signing files. You can also search for and export these files using `codesigndoc`.

1. Open the `Terminal`.

2. Enter the appropriate one-liner command, depending on your project type.
  * For an __Xcode__ project:

    `bash -l -c "$(curl -sfL https://raw.githubusercontent.com/bitrise-tools/codesigndoc/master/_scripts/install_wrap-xcode.sh)"
`
  * For a __Xamarin__ project:

    `bash -l -c "$(curl -sfL https://raw.githubusercontent.com/bitrise-tools/codesigndoc/master/_scripts/install_wrap-xamarin.sh)"`

1. Open your `Finder.app` and drag-and-drop your project's `.xcodeproj` or `.xcworkspace` file into the command line in your `Terminal`.

You can also install and run `codesigndoc` manually - for more information, check out the [tool's Readme](https://github.com/bitrise-tools/codesigndoc)!

!!! note "Troubleshooting: Ensure the correct state of the code"
    You get the most accurate result if you run `codesigndoc` on the same state of your
    repository/code which is available after a clean `git clone`, as that will
    be the state of the code after the build server checks it out (for example,
    you might have files on your Mac which are in `.gitignore`, so it exists
    on your Mac but not in the repository or after a `git clone` on a new Mac).

    So, for the best results, we recommend you to:
    1. __Do a clean `git clone` of the repository__ (into a new directory) on your Mac.

    2. Run `codesigndoc` in this directory (not in the directory
       where you usually work on the project).

!!! note "Troubleshooting: make sure you can export an IPA from Xcode.app"
    It's also advised to do a full Archive + Export (until you get a signed `.ipa`)
    of your project from `Xcode.app` first, and run `codesigndoc` __after that__.
    The reason is that `Xcode.app` might download or update profiles in the background
    during the IPA export. If you run `codesigndoc` after you exported an `.ipa`
    from Xcode, `codesigndoc` will able to collect all the files.

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


### iOS auto provisioning for Xcode projects

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

### Configure the appropriate Xcode Archive step to create the signed IPA

Once you have all your code signing files [collected](#collect-the-required-files-with-codesigndoc),
and you have the `Certificate and profile installer` and the `Xcode Archive & Export for iOS` or the `Xcode Archive for Mac` steps in the workflow,
you can start a build and get a signed IPA.

If you use Xcode 8/9 automatic code signing, the generated IPA by default will be a development signed IPA.
If you use Manual code signing, the default code signing type will be what you set in your
Xcode project for the Scheme/Configuration.

You can specify a distribution code signing type for either iOS or Mac apps.

1. Make sure you have either the `Xcode Archive for iOS` or the `Xcode Archive for Mac` step in the app's Workflow Editor, depending on your project type. Select your step.

1. Set the `Select method for export` input of the step to the type of code signing you want to use.

  If you use automatic provisioning, make sure it matches the value of the `Distribution type` input of the `iOS Auto Provisioning` step. The options are:  
  * `auto-detect` - please note that this option is deprecated and will be removed. We do not recommend using it. It selects the export method based on the Provisioning Profile embedded into the generated Xcode archive, and __NOT__ based on the uploaded codesigning files.
  * `app-store`
  * `ad-hoc`
  * `enterprise`
  * `development`.

1. Save the Workflow, and start a new build.

That's all. Xcode will auto select the right signing files based on your project's Bundle ID and
Team ID settings, and the Export Method you set.

If you want to sign the IPA with a different team's code signing files (e.g.
if you use your company's code signing for internal builds, but your client's
code signing files for App Store distribution), all you have to do is to set
the `The Developer Portal team to use for this export` option as well (in addition
to the `Select method for export`).


### Configure the Xamarin Archive step to create the signed IPA

Once you have all your code signing files [collected](#collect-the-required-files-with-codesigndoc), and you have the `Certificate and profile installer`, you can start a build and get a signed IPA.

To control what kind of code signing the IPA should be signed with, all you have to do is:

1. Make sure that you have the `Xamarin Archive` step in the app's Workflow Editor and select it.

1. Set the `Xamarin solution configuration` input of the step to the Xamarin project Configuration you want to use (e.g. `Release`).

1. Set the `Xamarin solution platform` input to `iPhone`.

You can control the code signing type in your Xamarin project by setting the
code signing configurations in Xamarin Studio.

_If you want to use more than one code signing type (for example, to create both Ad Hoc and App Store
signed apps), you should create more than one Release configuration in Xamarin Studio,
and set the separate configurations to the types you want to use (e.g. one to Ad Hoc,
and the second one to App Store)._

!!! note "Tip: Copy/clone an existing Release configuration"
    You can `Copy` the existing
    `Release` configuration in Xamarin Studio, to have an identical base configuration,
    where you only change the code signing settings. For example,
    `Copy` the `Release|iPhone` configuration with the name `ReleaseAppStore`,
    set the code signing to App Store for this `ReleaseAppStore` configuration,
    and specify this configuration as the `Xamarin solution configuration`
    input of the `Xamarin Archive` step.

    _Note: Don't forget to run `codesigndoc` again if you change code signing
    configurations in your Xamarin project, or to manually collect
    and upload the signing files required for the configurations
    you want to use!_
