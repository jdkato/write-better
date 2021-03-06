# ![logo](assets/images/quill-orange24.png) Write Better 

[![Build Status](https://travis-ci.org/justiceo/write-better.svg?branch=master)](https://travis-ci.org/justiceo/write-better)

An English language grammar checker for Google docs. A.K.A the chrome extension port of [Btford's WriteGood](https://github.com/btford/write-good) which is a bundling of different naive English language linters. I checked for grammatical errors on this README using WriteBetter ;)

![Sample Suggestions](assets/images/screenshot1.png)

### Features
* **Works Offline**: It comes packaged with all the resources needed to parse text, generate and display suggestions on Google docs and under 100Kb in size! See how to [inspect requests made by chrome extensions](https://www.howtogeek.com/302558/how-do-you-monitor-requests-made-by-a-google-chrome-extension/) if you're curious about what data other extensions are sending or requesting.
* **Privacy First**: The texts of your Google docs are never uploaded to a remote server or stored locally. All analysis happen offline and on demand in the browser. See what [can go wrong when extensions make copies of your data](https://gizmodo.com/grammarly-bug-let-snoops-read-everything-you-wrote-onli-1822740378). 
* **Open Source**: You can browse [the code here](https://github.com/justiceo/write-better), [modify and build it yourself](#build-the-extension-locally). It is a stringing of existing open source language libraries. See the [list of libraries](https://github.com/btford/write-good#checks) used.
* **It's Free!**: Sorry I needed to include this given that it costs about $20 to get a pro paper review on services like Fiverr or Grammarly.


#### Upcoming features:
* **Multi-Language Support**: Would allow users to run the checker on any language for which there is an open-source checker, like [Schrieb-gut](https://github.com/TimKam/schreib-gut) for German. 
* **Controlled Annoyance**: Grammar checkers always need to deal with false positive suggestions - it's the nature of English. Goal is to allow user to control confidence level of suggestions to display.
* **Other Writing Surfaces**: At the moment, the extension is [only activated](https://github.com/justiceo/write-better/blob/master/assets/manifest.json#L20) on https://docs.google.com. Generalizing it to other writing surfaces like email would require a bit more effort.


### Build the extension locally

1. Download the repo

```
git clone http://github.com/justiceo/write-better  
```

2. Install dependencies 

```
cd write-better && npm install  
```

3. Build the extension

```
gulp build
```
Extension directory would be in write-better/extension. See how to [load an unpacked extension](https://developer.chrome.com/extensions/getstarted#manifest) in chrome.

### Other tasks

4. Test the extension

```
gulp test
```

5. Start the build in live-reload mode

```
gulp
```

6. Create a release-able extension

```
git checkout -b release                         # Not necessary but preferable.
gulp clean                                      # Remove any artifacts from old builds and watch
gulp build                                      # Build the extension
zip -r extension.zip extension -x "*.DS_Store"  # Zip it up and upload to chrome dashboard.
```

### Debugging

1. Content script can be located in Chrome Devtools -> Sources -> Content Script. Ensure Console output level is VERBOSE
2. Background script can be located in Manage Extensions -> (Find loaded extension) -> Background page.
3. After a rebuild, reload extension using reload button on the extension card at chrome://extensions/

#### While awaiting a PR for these libraries, do:

1. Update node_modules/passive-voice/passive.js+184 to `var re = new RegExp('\\b(am|are|were|being|is|been|was|be)\\b[\\s|\\u200C]*([\\w]+ed|' + irregulars.join('|') + ')\\b', 'gi');`

2. Update node_modules/too-wordy/too-wordy.js+222-223 to 

```
const spacedWordyWords = wordyWords.map(w => w.replace(/ /g, '[\\b\\s\\u200C]*'))
const wordyRegex = new RegExp(`\\b(${spacedWordyWords.join('|')})\\b`, 'gi');
```

3. Update node_modules/no-cliches/cliches.js+701-702 to
```
const spacedCliches = cliches.map(w => w.replace(/ /g, '[\\b\\s\\u200C]*'))
const clicheRegex = new RegExp(`\\b(${spacedCliches.join('|')})\\b`, 'gi');
```

4. Update node_modules/write-good/lib/there-is.js+9 to `const startsWithThereIsRegex = new RegExp('^(\\s)*there[\\b\\s\\u200C]*(is|are)\\b', 'i');`


### Feedback
The only way I get feedback is when people complete this [anonymous form](https://forms.gle/LXBcvMG9Vt4fFUen8) or leave a review/comment on the extensions page. Please use them!

### Release Notes
#### v0.0.7
* Fix extension not working on large documents.
* Severally performance improvements and bug fixes.