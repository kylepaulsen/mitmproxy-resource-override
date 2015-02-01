# mitmproxy - Resource Override

A script for the proxy mitmproxy to help you gain full control of any website (through any browser) by redirecting traffic to specific files on your filesystem. This is similar to my [chrome extension](https://chrome.google.com/webstore/detail/resource-override/pkoacgokdfckfpndoffpifphamojphii).

# How to use
1. Install mitmproxy (see the other info section below if you have trouble)
2. Download this script.
3. Create a file called overrides.txt where you are going to run mitmproxy.
4. Put your override rules inside overrides.txt (See below for more details on this)
5. Run mitmproxy (--anticache is recommended):

<!-- Markdown is stupid - need to use a comment to turn off list formatting. -->

    $ mitmproxy -s mitmResourceOverride.py
    or
    $ mitmproxy --anticache -s mitmResourceOverride.py


# overrides.txt
This is the file where you define your url replace rules. Each rule is on its own line.
The script will parse this file every time a request is made so you can change it without having to restart mitmproxy. Here is what one replace rule looks like:

```
http://example.com/*.js , mySrcFolder/*.js
```

A rule is made up of a url, comma, and lastly a file path. The file paths are relative to where you start mitmproxy (and where overrides.txt is). The url and file path can make use of \* (star) globs. The star syntax in the paths is the same as my [chrome extension](https://chrome.google.com/webstore/detail/resource-override/pkoacgokdfckfpndoffpifphamojphii). A star in the url will greedily capture as much text as it can and store it so you can use it again in the file path. You can use consecutive stars ( \*\* , \*\*\* ) to store text under a different "name". See the table below for examples:

| Rule (URL , File Path)                                 | Requested URL                 | File Path That Is Used As Response |
|--------------------------------------------------------|-------------------------------|------------------------------------|
| http://example.com , src/index.html                    | http://example.com            | src/index.html                     |
| http://example.com/* , src/\*                          | http://example.com/foo.js     | src/foo.js                         |
| \*example.com/js/\*\* , someFolder/js/\*\*             | https://example.com/js/bar.js | someFolder/js/bar.js               |
| http://example.com/*/**/foo.js , src/\*\*/\*/foo.js    | http://example.com/a/b/foo.js | src/b/a/foo.js                     |
| \*cool\*js/\*\* , myDir/\*\*                           | http://wow.cool.com/js/a.js   | myDir/a.js                         |

Table rows 3 and 5 show how you don't have to use all the star globs in the file path allowing you to throw away parts of a url.

Table row 4 shows how you can reverse the order of the url path in your file path.

# Other Info About mitmproxy

Install mitmproxy following the instructions here: https://mitmproxy.org/doc/install.html

OR TL;DR, Mac and Linux: Install pip and then run pip install mitmproxy

You might need to install some other dependencies if it fails (Read the error logs).

**mitmproxy for Windows:** One way I was able to use mitmproxy on windows was to install it using Ubuntu Server in a virtual machine. Virtual Box can forward ports from the guest VM so that you can connect to the proxy using that port. See the bottom of this page to see how to port forward on Virtual Box: https://github.com/CenturyLinkLabs/panamax-ui/wiki/How-To%3A-Port-Forwarding-on-VirtualBox

You can also set up shared folders with Virtual Box (after you install guest additions) so you can access your host machine's files. Go to the virtual machine's settings and look for "Shared Folders"

You may want to add the Certificate Authority cert files to your computers trusted CAs. The certs are usally in ~/.mitmproxy . Google on how to do this.

# License

MIT
