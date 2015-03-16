# Run MPF2 under Ubuntu 12.04 x86\_64 #

The Motion Planning Framework 2 (MPF2) supersedes the original MPF. This brief guide explains how to run MPF2 under Ubuntu 12.04 x86\_64 Linux. MPF uses the .NET framework, so the Mono development tools are required under Linux:

```
sudo apt-get install mono mono-complete monodevelop
```

Open the solution with the MonoDevelop GUI:
  1. run monodevelop GUI
  1. open `nmf/planning/motion/MPF2/MPF_Shell/MPF_GUI/MPF_GUI.csproj`
  1. click **OK** for **"Reference to unknown project 'MPF\_Engine' ignored"**
  1. right-click **"Solution MPF\_GUI"**, choose **"Add Existing Project..."**, select `MPF2/MPF_Shell/MPF_Engine/MPF_Engine.csproj`
  1. right-click `MPF_GUI`, select **"Options"**
  1. click **"General"** under **"Build"** in the left pane
  1. change **"Target framework"** to **"Mono/.NET 4.0"**
  1. click **OK**
  1. click **"Build"**, **"Build All"**
  1. run it from the **Run** menu! a GUI should window should appear.