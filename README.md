# PhoneGap test project with 3.0 plugin

## Current issues

I'm up to the point where I get this error:

    D/PluginManager( 9530): exec() call to unknown plugin: WebSocket
    
## Background

This is how I got where I am:

```
cordova platform add android
cordova plugin add https://github.com/remy/phonegap-websocket.git
```

I took an existing working plugin for Android that worked on 2.8.x and tried to upgrade to 3.x.

## Issues I've hit

### cordova.api.xxx

This wouldn't work:

    import org.apache.cordova.api.CallbackContext

Someone mentioned on twitter that removing the `.api` would fix it (so I applied this across the board):

    import org.apache.cordova.CallbackContext

Now the build process works that I call via

    cordova build android

### cordova.exec in the JS

The docs still refer to `cordova.exec(…)`, but when I used [jsconsole remote inspector](http://jsconsole.com/remote.html) to look at the `cordova` object, I found there was no `exec` method.

Checking the official plugins for Camera, etc, I see:

    var exec = require('cordova/exec')
    
So I changed all refs of `cordova.exec` to use a locally required version of the method.

