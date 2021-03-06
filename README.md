Install Scribus w/ Homebrew Package Manager
================
##### **[Read this important note before continuing](https://github.com/scribusproject/homebrew-scribus#important-note-from-the-scribus-core-devs)**  

Install Scribus trunk (AKA the bleeding edge version) via [Homebrew](http://brew.sh), a MacOSX package manager.
The formula used to download the Scribus trunk is in what is called a Homebrew 'tap'. **'Tap' is homebrew-speak for a git repository containing extra formulae** outside of the main Homebrew repo. The Scribus tap is hosted on the [official Scribus github repo](https://github.com/scribusproject/homebrew-scribus).  
**Note**: This tap installs the latest version of 1.5.x trunk (there is no 1.4.x unless someone wants to make it.) Use this at your own risk. Scribus team is not responsible for any loss of data or breakage of equipment. 

Requirements
============
* An Intel CPU
* OSX 10.9+
* Xcode (Command Line Tools or full Xcode install)
* A Bourne-compatible shell for installation (e.g. bash or zsh)
* No macports<sup>[1](#1)</sup> installed

Installation
=====
**Install Xcode<sup>[2](#2)</sup>**  
Several options to choose from:  
  1. Command Line Tools for Xcode: `xcode-select --install`  
  2. Download [Xcode using iTunes](https://itunes.apple.com/us/app/xcode/id497799835)  
  3. Download Xcode directly from https://developer.apple.com/downloads  

**Install Homebrew Package Manager**   
1. Open Terminal.app or [iTerm](http://iterm2.com)  
2. ```ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```  <sup>[3](#3)</sup>  
![homebrew-install](https://cloud.githubusercontent.com/assets/4140247/8355316/32b5c0fa-1b14-11e5-9393-2ed0588983d6.gif)


**Install Scribus and it's dependencies**  
1. ``brew tap scribusproject/scribus`` will copy the formula from the Scribus github repo AND from now on check the repo formula for updates everytime brew update is invoked.  
2. ``brew install --HEAD scribus --verbose`` will build Scribus.. This will take some time.  
3. ``brew linkapps scribus --local`` will symlink Scribus.app to ~/Applications  

## Usage 
**Run Scribus**  
Several options to choose from:  
* You can run from Finder: ```⌘+[space]``` then type ```Scribus``` and choose the one in ~/Applications  
* You can run from command line:  ``~/Applications/Scribus.app/Contents/MacOS/Scribus``  

## Use Bleeding Edge Scribus
So now you can follow the development of Scribus. In order to use the latest code you need to follow these steps:  
* Open terminal or iTerm
* Run ```brew update && brew upgrade --all``` to make sure homebrew is up to date
* Run ```brew reinstall --HEAD --verbose scribus --verbose```

## Advanced Usage 
**Run HEAD with a specific Patch**  
Sometimes you may run in to a situation where you'd like to test or include a patch to Scribus. You can do that thus:
* Within the Formula block add a line with ```Patch :DATA```  
* Then **append the patch to the end** of the formula. 
By doing:
```
__END__
[add the patch here]  
```
See [Example](https://gist.github.com/luzpaz/69c00f2a8516b5462614) or check out:  
```
class Scribuspatch < Formula
  homepage 'http://wiki.scribus.net/canvas/Scribus_and_Homebrew'
  version '0.9'
  head 'svn://scribus.net//trunk/Scribus', :using => ScribusDownloadStrategy
  depends_on 'xz'
......
   patch :DATA

   def install
......
    system "cmake", ".", *args
......
  end
end
__END__
diff --git a/scribus/scribus.cpp b/scribus/scribus.cpp
index 966362a..a6a66b6 100644
--- a/scribus/scribus.cpp
+++ b/scribus/scribus.cpp
@@ -6539,6 +6539,7 @@ void ScribusMainWindow::slotPrefsOrg()
 	slotSelect();
 	struct ApplicationPrefs oldPrefs(prefsManager->appPrefs);
 	PreferencesDialog prefsDialog(this, oldPrefs);
+  prefsDialog.setWindowModality( Qt::WindowModal );
 	int prefsResult=prefsDialog.exec();
 	if (prefsResult==QDialog::Accepted)
 	{
```


## Important Note (from the Scribus core devs)

*  Don't use 1.5 for production, don't rely on it to work day to day. Use stable version (1.4.5) for that.
*  If you ignore the above, be prepared to stay on a version that works for you. There's normally no
     reason to change every day.
*  No bugs for homebrew solutions in the Scribus tracker. Use the [Issue Tracker](https://github.com/scribusproject/homebrew-scribus/issues) for that.
*  Trunk is not backward compatible, in other words, back up any older Scribus formats before you open them using trunk. (example: if you made a file using v1.5.0 and then opened it up with a trunk build do not expect it to work correctly again in 1.5.0) 
*  Trunk will be subject to be broken by the devs as it goes through the development process. 
*  Use this at your own risk. 

### Other Important Notes (non-dev related)

* If you have macports installed most likely you will run in to issues when using Homebrew. Therefore you will need to choose between one or the other. It's not recommended to run 2 different package managers on the same machine. 

Help Report Bugs
================
Since you are running Scribus trunk you can help Scribus development by making bug reports informative by generating backtraces of the bugs you encounter.  
  1. From the CLI invoke the debugger: ```lldb ~/Applications/Scribus.app/Contents/MacOS/Scribus```  
     The CLI will respond with:  
     ```(lldb) target create "/Users/snafu/Applications/Scribus.app/Contents/MacOS/Scribus"```  
     ```Current executable set to '/Users/snafu/Applications/Scribus.app/Contents/MacOS/Scribus' (x86_64).```  
  2. When lldb prompts you: type ``run`` to invoke Scribus from **within** debugger.  
     The CLI responds:  
     ```Process ##### launched: '/Users/snafu/Applications/Scribus.app/Contents/MacOS/Scribus' (x86_64)```  
  ![invoke-scribus-with-lldb](https://cloud.githubusercontent.com/assets/4140247/10736714/7443d1e6-7bdc-11e5-9d57-c3c71f04b0e5.gif)
  3. Switch back to Scribus interface and use it methodically to trigger the bug  
  4. Switch back to CLI and generate a backtrace by typing: ```bt```

  ![invoke-scribus-with-lldb-pt2](https://cloud.githubusercontent.com/assets/4140247/10736831/12ccd25e-7bdd-11e5-9652-1a0224f09f72.gif)

  5. Copy/Paste backtrace in to a bug report along with step by step instructions on how to trigger bug at http://bugs.scribus.net See [example bug report](http://bugs.scribus.net/view.php?id=13026).  

Todo
====
* https://github.com/scribusproject/homebrew-scribus/issues
- [ ] Make a legit Homebrew formula for 1.6 [WIP](https://gist.github.com/luzpaz/647414b7470b9e78b851)

Footnotes
====
<a name="1"><sup>1</sup></a> Homebrew will complain about libraries that belong to macports. At this time, it is recommended not to have 2 package managers installed on the same machine.  
<a name="2"><sup>2</sup></a> Most formulae require a compiler. A handful require a full Xcode
    installation. You can install Xcode, the CLT, or both; Homebrew
    supports all three configurations. Downloading Xcode may require an Apple Developer account on older versions of OS X. Sign up for free [here](https://developer.apple.com/register/index.action).  
<a name="3"><sup>3</sup></a> See http://brew.sh
