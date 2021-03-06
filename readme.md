[Read more and comment on my blog...](https://kwizcom.blogspot.ca/2017/08/spfx-isv-insight-to-microsofts-latest.html)

# Code demo key points:

1. The demo has 4 projects:
* KODemo (SPFx v1.1)
* ReactDemo (SPFx v1.4)
* SharedCode
* kwizcom-license
1. Two SPFx projects, one using Knockout and the other using React to render. One local package, and one local folder with some shared code.
1. check our launch.json for debug settings. 4 configurations, 2 for each project.

# Initial Setup
1. run npm install on all root folders
1. Go to "SharedCode" folder, run:
when building kodemo
```
    npm install @microsoft/sp-core-library@~1.1.0
    npm install @microsoft/sp-webpart-base@~1.1.1
```
when building reactdemo
```
    npm install @microsoft/sp-core-library@~1.4.0
    npm install @microsoft/sp-webpart-base@~1.4.0
```
> Note: delete node_modules when switching versions, before running npm install.

# KODemo
Contains an SPFx WebPart using Knockout to render the UI.
It uses SharedCode directly from a relative path, bundled into the WebPart.
It included kwizcom-license npm package, and bundles it as well.
Cannot run on local workbench since it connects to Microsoft Graph API.

Also, check out some build customizations in gulpfile.js
I included 2 important fixes:
- the fix for KnockOut templates that use containerless bindings, so that HTML comments are preserved
- controlling the production bundle file name, dropping the hash and replacing it with the version number
Read more about it: http://kwizcom.blogspot.ca/2017/08/controlling-spfx-bundle-file-name-in.html

# ReactDemo
Contains an SPFx WebPart using React to render the UI.
It uses SharedCode directly from a relative path, bundled into the WebPart.
It included kwizcom-license npm package, loading it as an external package from CDN: 
https://kwizcom.sharepoint.com/SITES/S/CDN/License.js

Note: as of SPFx 1.4 we can now flag to remove the hash from the bundle file name. See excludeHashFromFileNames used in gulpfile.js

## Custom property
The react demo has a custom property sample featuring a color picker.

## Multiple web part definitions
See ReactWebPartWebPart.manifest.json for preconfiguredEntries, a second instance with different default properties was added.

# SharedCode
This is just a folder with some code we want to share.
We include tsconfig.json since we want to work with TypeScript.
run npm install to get the packages we need. Since there is no package.json file -
```
    npm install @microsoft/sp-core-library@~1.1.0
    npm install @microsoft/sp-webpart-base@~1.1.1
```
or when using SPFx v1.4:
```
    npm install @microsoft/sp-core-library@~1.4.0
    npm install @microsoft/sp-webpart-base@~1.4.0
```
run tsc when you want to build the JS files after updating the TypeScript.
    note: I cannot get rid of the type mismatch error. have to send variables as type any.

# kwizcom-license
This simulates a folder that it its own npm package.
We run npm init to create a package, and keep versioning updates.
We add it to our other web parts as packages in hte package.json dependencies, using a local relative folder path:
    "kwizcom-license": "file:../kwizcom-license"
and use npm install and npm update to add it.
This package published the output JS file to a CDN:
https://kwizcom.sharepoint.com/SITES/S/CDN/License.js
and can be loaded as an external (and not bundled)


# Updates
For updates and news, visit https://github.com/KWizCom/SPFxDemo/edit/master/Updates.md
