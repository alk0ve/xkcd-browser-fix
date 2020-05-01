# xkcd-browser-fix

I'm a big fan of xkcd comics and I'm mostly viewing it using the Browser for XKCD (https://play.google.com/store/apps/details?id=com.floern.xkcd&hl=en), which is excellent. At some point, however, its explain-xkcd link stopped working (you could still open it in an external browser window) due to a redirect from HTTP to HTTPS that the internal browser couldn't handle (for a reason that escapes me). 
I also noticed that this was a known issue, and that the Browser for XKCD app wasn't updated for at least a couple of years, so I took a deeper look, and it turns out that all you need to fix is to update the URL used by the app: it originally used "http://www.explainxkcd.com/wiki/index.php?title=", followed by a strip number, and all you need to do is to change the protocol to HTTPS.

The exact steps are below, but the end product is in the Releases tab :)

1. Download the app's APK, for instance from apkpure.com: https://apkpure.com/store/apps/details?id=com.floern.xkcd
2. Download Apktool: https://ibotpeaches.github.io/Apktool/install/
3. Unpack the APK file using apktool: apktool d -o <output folder> <apk-file>
4. The explainxkcd.com URLs appear in \smali\com\floern\xkcd\comic\ExplainActivity.smali, and they are all http://www.explainxkcd.com/wiki/index.php?title= to which the app appends the strip number (verified by using Packet Capture app that sets up a VPN), which leads to a 301 (Moved Permanently) error code, redirecting into the HTTPS version of the same address
5. Change all the URLs in ExplainActivity.smali that mention explainxkcd.com to use HTTPS
6. Repack the APK using apktool
7. Sign the APK using Keytool & Jarsigner (both in JDK) - useful links:
  7.1. https://stackoverflow.com/questions/10930331/how-to-sign-an-already-compiled-apk
	7.2. https://docs.oracle.com/javase/6/docs/technotes/tools/solaris/keytool.html
	7.3. https://docs.oracle.com/javase/6/docs/technotes/tools/solaris/jarsigner.html
8. Zipalign the signed APK - useful link:
	8.1. https://stackoverflow.com/questions/36916462/how-to-zipalign-the-apk-file-in-windows
9. Install the APK on your phone
9. Profit :)
