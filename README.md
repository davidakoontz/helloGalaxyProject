# HelloGalaxyProject - an upgrade to Hello World - using Skip.tools to build on iOS & Android, also!
### One SwiftUI source tree -> Two Apps (iPhone & Android).  With Skip.tools magic!

This is a [Skip](https://skip.tools) dual-platform app project.
It builds a native app for both iOS and Android.

## Building

This project is both a stand-alone Swift Package Manager module,
as well as an Xcode project that builds and transpiles the project
into a Kotlin Gradle project for Android using the Skip plugin.

Building the module requires that Skip be installed using
[Homebrew](https://brew.sh) with `brew install skiptools/skip/skip`.

This will also install the necessary transpiler prerequisites:
Kotlin, Gradle, and the Android build tools.

Installation prerequisites can be confirmed by running `skip checkup`.

## Testing

The module can be tested using the standard `swift test` command
or by running the test target for the macOS destination in Xcode,
which will run the Swift tests as well as the transpiled
Kotlin JUnit tests in the Robolectric Android simulation environment.

Parity testing can be performed with `skip test`,
which will output a table of the test results for both platforms.

## Running

Xcode and Android Studio must be downloaded and installed in order to
run the app in the iOS simulator / Android emulator.
An Android emulator must already be running, which can be launched from 
Android Studio's Device Manager.

To run both the Swift and Kotlin apps simultaneously, 
launch the galaxyModuleApp target from Xcode.
A build phases runs the "Launch Android APK" script that
will deploy the transpiled app a running Android emulator or connected device.
Logging output for the iOS app can be viewed in the Xcode console, and in
Android Studio's logcat tab for the transpiled Kotlin app.

# UA Conference Presentation - Dec 2024 - by David Koontz

## From Hello World to a Galaxy on iOS & Android.

One code base - dual platforms (iOS & Android) - one App running on many platforms is a future here NOW.

I’m using the awesome trans-compiler of Skip.tools to build an App on iOS using SwiftUI code and concurrently building an Android (Kotlin) App.  I can show you how and give you a few tips to jump in and get started.  Ready?

Hello World is a great concept - it is just that it is limited to ONE World!  It’s time we expanded our cosmology.  In the 21st century we programmers have to deal with the many worlds of a Galaxy.  It is way past time to upgrade Hello World.

Introducing Hello Galaxy - a dual platform stepping stone to the Galaxy of many possible worlds.

What should the program include?  

Hello Galaxy Program

Start from a simple programming books table of contents.  
What in that list of concepts must we put into the Hello Galaxy program?

	✓	Language Syntax
	✓	Language Comments
	✓	Primitive Data Types (numbers, strings, booleans,)
	✓	Variables (variables vs constants vs literals)
	✓	Simple Data Types (Strings, Arrays, Tuple)
	✓	A List Data Type
	◦	Using a List in the typically ways (Map-Reduce Paradigm)
	◦	Using Loops (for, while, etc)
	◦	Flow of Control Statements (If then else; do while)
	◦	A Dictionary Data Type, Iterators or Looping over the Dictionary
	✓	User Input
	✓	Functions/Methods
	◦	Positional Parameters to functions
	◦	Local vs Global Scope of Access
	◦	Access Control Restrictions
	◦	Exception Handling
	✓	Classes / Objects / OOP foundations:
	◦	Abstraction
	◦	Encapsulation
	◦	Inheritance
	◦	Polymorphic message passing
	✓	API call to service & JSON handling of response
	◦	Code structures for organization - e.g. Modules
	◦	Unit & UI Tests
	✓	Multiple Platform Development - from a single source code.


Utilizing the awesome toolchain of Skip.tools we will upgrade their Hello World to Hello Galaxy an App that runs on both iOS (and maybe even MacOS) as well as Android.  And introduce you to the subtle art of the transpiler; e.g. converting Swift code into Kotlin code and the libraries that bridge the SwiftUI world to the Kotlin world.

I’m not going to reproduce the excellent Getting Started documentation and videos that you will find on the Skip.Tools site.  I’m going to start from the recommeneded Hello World project that installs from the skip init command line.  This project will be downloaded from the GitHub Repo and setup as your first commit and branching off point for a NEW project.

So assuming you have installed all the Skip prerecs ( Android Studio - Gradle - Homebrew - etc. )

You go to a command line in the folder you wish to hold your project.  I noticed that Skip doesn’t like me changing project folder names… so pick a good one.  Run the skip init command with options.

### Skip Docs state:  skip init --appid=bundle.id project-name AppName command.

Where you will replace the “my…” placeholders with a really cool name of your own!

### > skip init --appid=com.myCompany.myAppID myProject myApp


As of this time - skip init  - does not configure a source control repo… so the typical next step is to setup/configure a repo (I will use GitHub).  

Warning about names:  I’ve done this skip init so many times - and I’ve learned that SwiftUI & Kotlin/Java will have some incompatible names - if we are not careful.  Open the skip.env file and look for proper camel cased names,  whitespaces, and numbers, etc.  Also skip build system tends to embed the paths to executables and resources into its generated scripts… therefore likely to fail if you try to rename folders (I’ve tried and fail so many times).  After making this mistake … I typically start back at the beginning (skip init) and attempt to get it right.  This file contains the version numbers that you will want to update regularly.
See: https://skip.tools/docs/development-topics/#configuration-with-skipenv

If it’s been a few days… you might need to upgrade skip!  It’s a quickly changing platform… so stay current.

The project folder (helloProject) is going to be very different than the typical Xcode project… so let’s introduce these top level folders.  The obvious folder that jumps out is the Android folder - much of this folder is typically ignored.  It is the set of many Android_Studio files and resources (such as icon files).  The Darwin (a leftover name for iOS) can mostly be ignored also - this is where the Xcode project files live.  And ignore Fastlane also - it is a tool for doing multiplatfom deployments, and if I could have gotten it to work in the couple weeks I spent trying - I would have nice things to say about it.  That leaves us with just the Sources folder and Tests folder.

One issue I had very early on was wondering where to put my new source files… as one can find source files in just about all of these top-level project folders.  The answer is your new files belong in Sources and Tests folders.

Since we are starting from a working, building, transpiling, and running Hello World App… we should first get it running.  Which means quite a few preliminary requirements:  Start Xcode with the project file down in Darwin/galaxyModule.xcodeproj, (open Darwin/galaxyModule.xcodeproj).  This should launch Xcode and read in all the project related source files.  Skip requires an Android device be running while it transpiles SwiftUI to Kotlin and generates Java source files. For this we need Android Studio & it’s Device Manager to launch a Device simulator. 

But before you can build - several thing need to happen:
	- Launch Android Studio
	- Launch an Android Device (via Device Manager)
	- In Xcode select a device to deploy onto - iPhone 16 in the top header

	- Click the Xcode “play” button to build and run…  results in an ERROR.  “skipstone” is disabled
	- Resolve the Error by clicking the message … enabling the SkipStone plugin.

	- Click “Trust & Enable” for the SkipStone plugin.

	- Build AGAIN.  (in Xcode - “play” button)


Before you start changing/modifying files - let’s check this baseline project “Skip Hello World” into a GitHub Repo.

First go to the command line and the top-level project folder (I’ve not gotten Xcode to properly initialize a local repo - so I use the command line.

After using the command line for:  git init  &  git add .  <dot - here means current dir and it recursivly pick up every file>
Skip.Tools provided a good  exception list in .gitignore.

Now when I look in Xcode - Integrate > Commit  I see all the files being added [A] icon.  Stage All and Commit.

Now let’s setup a Remote Repo on the GitHub server (you have an account there right?).
On Xcode’s Source Control tab > Remotes > New “helloProject” Remote…

Then enter a pithy project description comment for the world to see.  I like to change the name of the Remote to “GitHub” from “origin.”  It makes it very clear where this remote Repo lives.

Then you should be able to push the original project files to GitHub.  Integrate > Push…


If you want this finished project - download it from: https://github.com/davidakoontz/helloGalaxyProject

At some point Xcode is going to complain about not having a ‘development team’ assigned in the project file.  This is typical when using a GitHub repo to start from.  Edit the project file > Signing & Capabilities > Signing :: Team.

While editing the project file - set the Display Name and the App Category in the Identity section along with Deployment Info Orientation check boxes of General tab.

This might be about all the configuration you need to get the Hello project running on both platforms.  Now it’s time to learn how to add source and resourcese to the project.

### Let’s make a small SwiftUI code change

Open the ContentView.swift file in Xcode, scorll down or search for “Home” and in  the .navigationTitle change the string to “Home List” - compile and run.  Look at both simulators iOS & Android… go to the Home tab and look for the title change.  There - that’s it.  That is how easy it can be to write two platform stacks with ONE SwiftUI source tree.


You should have the Apps running in both simulators and reflecting the ONE change.

### Let’s add an App Icon

I like to use the tool:  https://www.appicon.co
It generates iOS resources and Android resources - all from an initial 1024 x 1024 PNG image.

I’ve generated the resources for the App Icons.  I install the iOS app icon the typical way - copy the resources into the Assets.xcassets.

Skip does not migrate iOS Assets in the App folder ( see: https://skip.tools/docs/modules/skip-ui/#images ). But now we do this on Android.  It is different and this is what I found worked.  In Android Studio make sure you have the same project opened.

Reference:  https://developer.android.com/studio/write/create-app-icons

In Android Studion - open project [this is especally difficult - there is NO Project file!]
Some internet sources say click on the setting.gradle.kts file (there are multiple of these in the Android tree).  I’ve done something wrong so many time… and I’m not afraid of starting over from > skip init….

File > NEW > Image Asset

This opens a window: Configure Image Asset.  The Path: setting is what we wish to set to our new app icon.

In the Path setting click to load the file on disk….  (Using appicon.co  https://www.appicon.co one gets a Zip file - open it and the folder AppIcon is yet another folder AppIcon this folder holds android folder, Assets.xcassets, and 2 PNG original files named appstore & playstore.) 

I guess there is a way to use the whole “android” folder whick contains  mipmap-**** folders…  I think this is the stuff AStudio will generate…  I’ve NOT used them.

In the Configure Image Asset window.  Click on the Path folder…  select the playstore.png  file.
This loads the file in the display…  NEXT
then Generate.


### I think that’s it…

Let’s see if the App builds with the new icon?

Let’s add a new source file  - in Xcode ….




You now have TWO build systems running kinda currently - the Xcode Swift build & the Android Gradle build… things are bound to get out of sync… some time to fix errors - I close down the whole world - exit Xcode and reStart Xcode… and every now and then - this is a fix.



Deploying to the App Store & Play Store - are another hurdle… for another time.




## Frequent Issues and Resolutions:

Do NOT rename your project or enclosing folders.  Skip has used the absolute path to it’s resources.

Do NOT use a comment with “// SKIP some note to remember” the all caps version of that comment will be found by the Skip Transpiler and it will attempt to follow the unknow command.  Use mixed case in comments:  // Skip is an acceptable comment.

Do Start from the > skip init Hello World project.  Copy in your swift files and resources into Sources folder.  Edit the ContentsView.swift to make it your own.  You may need to add Swift environment objects into the *App.swift code.

If you get complations errors because you are trying to use a SwiftUI API that has yet to be ported.  You can typically workaround the needed API via simplification and older iOS16 code bases.

Check your Xcode Bundle Idenetifier - I’ve had problems with no allowable characters and poorly formated names.

I’ve seen cases where the module name gets translated to a poorly named Java path (with spaces).

The skip.env file contains environment varables - I’ve removed the dot in ANDROID_PACKAGE_NAME - sometimes I ignore this naming issue.
// The package name for the Android entry point, referenced by the AndroidManifest.xml
ANDROID_PACKAGE_NAME = galaxy.module

There is a hidden folder “.build” that is used by the Skip Transpiler to cache generated Kotlin files…  sometimes it needs to be deleted and there is no gradle clean command.  In Finder <Shift> <Command> .  (shift-command-period) turns on hidden folder/files view.


The Skip tools also use the Xcode DerivedData folder on your Mac… it may need to be cleared form time to time.
Xcode > Product > Show Build folder in Finder.
I use the command line:  rm -rf DerivedData


Check for Skip upgrades often:  > skip upgrade

Sometimes one needs to upgrade the SPM from inside of Xcode:
Xcode :  File > Packages > Upgrade to latest package versions

Skip often uses long absolute paths to it’s executable that has been copied from your Homebrew installation to the local project .build folder.  This causes upgrading issues and incompatiblity problems in my opinion.  It is just one of the reasons to clean the caches (.build, DerivedData, Build Folder, etc.).


Using compiler directives #if SKIP to customize for Android platform.

#if SKIP
print("Android")
#else 
print(“iOS”)
#endif

Note:  in the case of the Android branch - the code is written in Swift. 
“Android-only code in #if SKIP blocks is still parsed by Xcode, so it must have valid Swift syntax. But it is excluded from your iOS build and invisible to the Swift compiler, which allows it to make any syntactically valid API call without causing Xcode errors.”

See Also:  https://skip.tools/docs/platformcustomization/

Lots of unique Koltin and Java code can be used via the conditional compiler directives: 

#if SKIP
let dateFormat = java.text.SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss'Z'", java.util.Locale.getDefault())
dateFormat.timeZone = java.util.TimeZone.getTimeZone("GMT")
return dateFormat.format(java.util.Date())
#else
... equivalent iOS code ...
#endif

### Android Studio Issues:

The project is using an incompatible version (AGP 8.7.0) of the Android Gradle plugin. Latest supported version is AGP 8.5.2

Update to it inside Android Studio by clicking Help > Check for updates (Android Studio > Check for updates on macOS)


updating the Android Studio App itself is done from another app called Toolbox.




## References:

Why build for Android; Also?  https://www.theswiftdojo.com/why-build-for-android-also

GitHub Hello Galaxy project:  https://github.com/davidakoontz/helloProject

Skip Tools  https://skip.tools  : Video Tour https://skip.tools/tour/  : Advantages https://skip.tools/docs/#advantages
						   : GitHub Discussions https://github.com/orgs/skiptools/discussions/
						   : Slack  https://skip.tools/slack/

Skip Teaser reel :  https://skip.tools/tour/teaser/  

Skip Webinar : https://skip.tools/tour/webinar-2024-05-07/
