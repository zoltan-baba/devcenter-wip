## Configure the appropriate Xcode Archive step to create the signed IPA

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
