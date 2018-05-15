## Configure the Xamarin Archive step to create the signed IPA

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
