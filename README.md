I believe I have successfully managed to make a phonegap 3.0.0 port of this code, I'm working on bringing a release to the plugin soon...

# Cordova/PhoneGap 3.0.0+ sqlitePlugin - Windows Phone 8+ version

Native interface to sqlite in a Cordova/PhoneGap plugin, working to follow the HTML5 Web SQL API as close as possible.
License for this version: MIT or Apache

## Highlights

 - Supports new phonegap 3.0.0 plugin architecture
 - Keeps sqlite database in a user data location that is known and can be reconfigured
 - Drop-in replacement for HTML5 SQL API, the only change is window.openDatabase() --> sqlitePlugin.openDatabase()
 - batch processing optimizations
 - No 5MB maximum, more information at: http://www.sqlite.org/limits.html

## Known Issues

 - Drop table is not working, looks like a bug in the native SQL library. To get around this we empty the table instead of dropping it.

## Support

Raise an issue, but I'm a busy guy, don't expect a response quickly or at all. I post this code for the benefit of the community and hope you can kinda figure it out, sorry I just don't have enough time to devote to supporting this.

I should probably also declare c#.net and visual studio are a language and environment that I don't have a huge amount of experience with (in fact at time of this commit I've only been using phonegap 3 for a few days), so please excuse me if some of my techniques to get this to work are a little unusual. 

## Thanks

To the original project:

https://github.com/marcucio/Cordova-WP-SqlitePlugin

"Dev Girl":

http://devgirl.org/2013/09/05/phonegap-3-0-stuff-you-should-know/

http://devgirl.org/2013/09/17/how-to-write-a-phonegap-3-0-plugin-for-android/

## Steps to get a test application off the ground - (WORK IN PROGRESS)

 - I assume you know how to install node.js, phonegap and cordova (yes, I install "both" the phonegap and cordova CLI)
 - Remember to install git cli to your path
 - Remember to add msbuild.exe (32bit .net 4 framework) to your path or phonegap build commands won't work
 - Create a new phonegap project: phonegap create dbTestApp --name "dbTestApp" --id "com.example.dbTestApp"
 - cd into your new dbTestApp folder
 - Create a wp8 platform: phonegap build wp8
 - Download sql library

 I'm using this sql library...

 https://code.google.com/p/csharp-sqlite/ 
 
 download here:
 
 https://code.google.com/p/csharp-sqlite/downloads/detail?name=csharp-sqlite_3_7_7_1_71.zip
 
 I believe this is also on nuget, but I couldn't figure out how to make the many nuget sqlite libraries work, interestingly there seems to be an official microsoft slqite library.
 
 Extract the zip file to wherever you like.
 
 - Open up the sql library solution for windows phone and compile it:
 
 Community.CsharpSqlite.WinPhone\Community.CsharpSqlite.WinPhone.csproj
 	
 VS2012 will ask if you want to do a one way upgrade of the project, click OK
 
 It will then give you a security warning, click OK
 
 Build the Solution
  
 - Add sql library to phonegap project
 
 Find the compile dll and pdb inside Community.CsharpSqlite.WinPhone\Bin\Debug\
 
 Place the dll and pdb into your phonegap solution's packages folder like so... ROOT\platforms\wp8\packages\Community.CsharpSqlite.WinPhone\ (create the folders if they doesn't exist)
 
 (I don't know if this makes any difference but at this step I opened my dbTestApp.sln file in VS2012 to see if anything had happened, on closing VS2012 it created a dbTestApp.v11.suo file)
 
  - Add in plugin references and compile flags
 
 In your wp8 phonegap project under ROOT/platforms/wp8 open the dbTestApp.csproj in a text editor of your choosing (I don't know how to do this "properly" using VS2012)
 
 Find the <property group> tags that have attributes Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' " and  Condition=" '$(Configuration)|$(Platform)' == 'Release|AnyCPU' "
 
 Inside those property group tags, change both define constants elements from
 
 <DefineConstants>TRACE;DEBUG;SILVERLIGHT;WINDOWS_PHONE;WP8</DefineConstants>
 
 to
 
 <DefineConstants>TRACE;DEBUG;SILVERLIGHT;WINDOWS_PHONE;WP8;USE_WP8_NATIVE_SQLITE</DefineConstants> 
 
 (Remove "DEBUG" from the "Release" DefineConstants, i.e. <DefineConstants>TRACE;SILVERLIGHT;WINDOWS_PHONE;WP8;USE_WP8_NATIVE_SQLITE</DefineConstants>)
  
 At the bottom of the file you'll find some Reference elements inside ItemGroup elements, add this line...
 
    <ItemGroup>
	    <Reference Include="Community.CsharpSqlite.WinPhone">
	       <HintPath>packages\Community.CsharpSqlite.WinPhone\Community.CsharpSqlite.WinPhone.dll</HintPath>
	    </Reference>
    </ItemGroup>
 
 (Once again I tried opening the .sln file, this time you should find in your project under dbTestApp > References a link to Community.CshaprSqlite.WinPhone)
 
 - Download/install plugin: phonegap local plugin add http://git.com/<url TBC>
 
 The following should change in your phonegap project...
 
 ADDED: ROOT\plugins\SQLitePlugin
 
 ADDED: ROOT\platforms\wp8\Plugins\SQLitePlugin
 
 ADDED: ROOT\platforms\wp8\www\plugins\SQLitePlugin
 
 (...plus a couple of other bits and bobs in config files)
 
 - Start using sqlite :)

## Gotchas

 - Adding and removing the plugin seems to duplicate the cordova.js and sqllite plugin files inside the VS2012 project, simply remove the duplicates from the project if you have troubles
 - Using this version of sqlite is syntatically different to the built in sqlite in android and ios, I've seen people complain this plugin has problems with multiple transactions and the like, this is NOT the case, you are using incorrect syntax if you are finding this. The main differences are you must return the database object of window.openDatabase and then use it inside a function at the end of the window.openDatabase command, also, I believe what functions get called at the end of a transaction versus the end of an executesql seems to work differently. I will try to give an example in time...