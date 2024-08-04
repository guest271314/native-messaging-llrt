Amazon Web Services - Labs LLRT (Low Latency Runtime) Native Messaging host

Installation and usage on Chrome and Chromium

1. Navigate to `chrome://extensions`.
2. Toggle `Developer mode`.
3. Click `Load unpacked`.
4. Select `native-messaging-llrt` folder.
5. Note the generated extension ID.
6. Open `nm_llrt.json` in a text editor, set `"path"` to absolute path of `nm_llrt.js` and `chrome-extension://<ID>/` using ID from 5 in `"allowed_origins"` array. 
7. Copy the file to Chrome or Chromium configuration folder, e.g., Chromium on \*nix `~/.config/chromium/NativeMessagingHosts`; Chrome dev channel on \*nix `~/.config/google-chrome-unstable/NativeMessagingHosts`; and similar for Chrome For Testing.
8. Make sure `nm_llrt.js` is executable. See [Notes](https://github.com/guest271314/native-messaging-llrt#notes) for why Bash or QuickJS are used to read standard input stream to `llrt`. 

9. To test click `service worker` link in panel of unpacked extension which is DevTools for background.js in MV3 `ServiceWorker`, observe echo'ed message from `d8` Native Messaging host. The communication mechanism can be extended to run `llrt` from any arbitrary Web page using various means, including, but not limited to utilizing `"externally_connectable"` to message to and from the `ServiceWorker` on specific Web pages over IPC; `"web_accessible_resources"` to append an extension `iframe` to any document and use `postMessage()` to transfer messages between browsing contexts; an offscreen document or side-panel document to connect to the host and transfer messages back and forth to the arbitrary Web page in the browser, et al.

### Notes

Standard input and standard output are not specified by ECMA-262. 

There does not appear to be a straigtforward way to read standard input to `llrt`.

We use [`pgrep`](https://man7.org/linux/man-pages/man1/pgrep.1.html) command to get the PID of the current process, then or GNU Coreutils [`head`](https://www.gnu.org/software/coreutils/manual/html_node/head-invocation.html),
[`dd`](https://www.gnu.org/software/coreutils/manual/html_node/dd-invocation.html#dd-invocation) command, or [QuickJS](https://bellard.org/quickjs/quickjs.html) to read `/proc/$@/fd/0`, then echo the STDIN to the current `llrt` process to `llrt`. 

### Compatibility

For differences between OS and browser implementations see [Chrome incompatibilities](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Chrome_incompatibilities#native_messaging).

# License
Do What the Fuck You Want to Public License [WTFPLv2](http://www.wtfpl.net/about/)
