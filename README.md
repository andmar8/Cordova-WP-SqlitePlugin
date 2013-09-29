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

 - Drop table is not working, looks like a bug in the native SQL library. To get around this we empty the tabe instead of dropping it.

## Support

Raise an issue, but I'm a busy guy, don't expect a response quickly or at all. I post this code for the benefit of the community and hope you can kinda figure it out, sorry I just don't have enough time to devote to supporting this.

## Steps to get a test application off the ground

 - Create a new phonegap project
 - More steps soon...