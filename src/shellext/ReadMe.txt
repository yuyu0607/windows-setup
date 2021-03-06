Open the .vcxproj file in any version of Visual Studio from 2010
onwards. Since 2010 is the lowest supported version and relies on the
v100 toolset, you may have to upgrade the project file to use the
toolset installed with your version. You do not need to save the
IDE-created .sln file, but if you do it is git-ignored anyway. If you
wish to contribute, please read the CONTRIBUTING section below to
understand how property changes must be made.

When you build the shell extension the dll will be found at one of
the following locations, depending on your build environment:

    Win32\Debug\shellext32.dll
    Win32\Release\shellext32.dll

    x64\Debug\shellext64.dll
    x64\Release\shellext64.dll

To install the shell extension, cd to an above directory and run:
    regsvr32 shellext32.dll  (or shellext64.dll)

To uninstall the shell extension run:
    regsvr32 /u shellext32.dll   (or shellext64.dll)

Note that when you install your dll, you will overwrite the registry
entry for the installed version of the extension (if present). To
re-install it, run regsvr32 from the ComposerSetup installaton directory.
For example:

    cd C:\Program Files\ComposerSetup
    regsvr32 shellext32.dll

You must close any File Explorer windows (then reopen them) for the
extension to be used.

CONTRIBUTING
The .vcxproj file must support different versions of Visual Studio,
so to ease maintainability we ask you to follow two basic rules:

    1) Property changes must be made using the property sheets. These
    can be edited from the Property Manager in the IDE or directly at:

        Properties\ComposerShell.Debug.Win32.props
        Properties\ComposerShell.Debug.x64.props
        Properties\ComposerShell.Release.Win32.props
        Properties\ComposerShell.Release.x64.props

    Ensure that you make relevant changes for all configurations.

    2) All references to PlatformToolset in the .vcxproj file must be
    changed to v100 before you submit your code. You will need to do
    this if your project file was upgraded.

One way of checking, before you make your final commit, is to open your
.vcxproj file in a text editor and look for
    <PropertyGroup Label="Configuration" ..> elements.
There will be 4 of these, one for each configuration, and you must change
the PlatformToolset value to v100 in each one.

Next look for unlabelled <PropertyGroup> elements, or any other
PropertyGroup elements that are not the
    <PropertyGroup Label="Globals"> block.
If any are found then you have not followed Rule #1. Transfer these
properties to the appropriate property sheet then remove the entire
blocks from the .vcxproj file.

Finally look for any <ItemDefinitionGroup> elements, which will only be
found if you have not followed Rule #1. Transfer these properties to
the appropriate property sheet then remove the entire blocks from the
.vcxproj file.

To summarize the .vcxproj file requirements:

    * there must be one <PropertyGroup Label="Globals"> ... </PropertyGroup>
    * there must be 4 <PropertyGroup Label="Configuration" ..> elements
      with PlatformToolset set to v100
    * there must be no other PropertyGroup elements
    * there must be no <ItemDefinitionGroup> elements
    * ToolsVersion must be "4.0", unless changing the lowest
      supported version
