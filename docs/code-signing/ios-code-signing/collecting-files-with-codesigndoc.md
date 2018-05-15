## Collecting code signing files with codesigndoc

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

You can also install and run `codesigndoc` manually. For more information, check out the [tool's Readme](https://github.com/bitrise-tools/codesigndoc)!

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
