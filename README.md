About
----- 

**drawio-desktop** is a diagramming and whiteboarding desktop app based on [Electron](https://electronjs.org/) that wraps the [core draw.io editor](https://github.com/jgraph/drawio).

Download built binaries from the [releases section](https://github.com/jgraph/drawio-desktop/releases).

**Can I use this app for free?** Yes, under the apache 2.0 license. If you don't change the code and accept it is provided "as-is", you can use it for any purpose.

Security
--------

draw.io Desktop is designed to be completely isolated from the Internet, apart from the update process. This checks github.com at startup for a newer version and downloads it from an AWS S3 bucket owned by Github. All JavaScript files are self-contained, the Content Security Policy forbids running remotely loaded JavaScript.

No diagram data is ever sent externally, nor do we send any analytics about app usage externally. This means certain functionality for which we do not have a JavaScript implementation do not work in the Desktop build, namely .vsd and Gliffy import.

Developing
----------

**draw.io** is a git submodule of **drawio-desktop**. To get both you need to clone recursively:

`git clone --recursive https://github.com/phamjef/SPOF-Analysis-Tool.git`

To run this:
1. `npm install` (in the root directory of this repo)
2. `npm install` (in the drawio directory of this repo `drawio/src/main/webapp`)
3. export DRAWIO_ENV=dev if you want to develop/debug in dev mode.
4. `npm start` _in the root directory of this repo_ runs the app. For debugging, use `npm start --enable-logging`.

To release:
1. Update the draw.io sub-module and push the change. Add version tag before pushing to origin.
2. Wait for the builds to complete (https://travis-ci.org/jgraph/drawio-desktop and https://ci.appveyor.com/project/davidjgraph/drawio-desktop)
3. Go to https://github.com/jgraph/drawio-desktop/releases, edit the preview release.
4. Download the windows exe and windows portable, sign them using `signtool sign /a /tr http://rfc3161timestamp.globalsign.com/advanced /td SHA256 c:/path/to/your/file.exe`
5. Re-upload signed file as `draw.io-windows-installer-x.y.z.exe` and `draw.io-windows-no-installer-x.y.z.exe`
6. Add release notes
7. Publish release

*Note*: In Windows release, when using both x64 and is32 as arch, the result is one big file with both archs. This is why we split them.

Local Storage and Session Storage is stored in the AppData folder:

- macOS: `~/Library/Application Support/draw.io`
- Windows: `C:\Users\<USER-NAME>\AppData\Roaming\draw.io\`

Single Point of Failure Functionality
-------------------------------------

For this specific build of drawio, you will need to have Apache Ant and Node.js installed
beforehand in order to build the project. Continuing from the installation and running from
the developing section, you will need to also use Apache Ant to update and rebuild the application
due to the min files. Please note that the build file for running Ant is located in SPOF-Analysis-Tool\drawio\etc\build
directory, so please make sure to switch to the correct directory

To update and rebuild with Ant:
1. `cd drawio/etc/build`Change to the "build" directory in drawio
2. `ant` (command to run ant in the build directory) 
3. `cd ..` (Return to the root directory of the project SPOF-Analysis-Tool)
4. `npm start` (in the root directory, runs the app with updates applied)

Open-source, not open-contribution
----------------------------------

[Similar to SQLite](https://www.sqlite.org/copyright.html), diagrams.net is open
source but closed to contributions.

The level of complexity of this project means that even simple changes 
can break a _lot_ of other moving parts. The amount of testing required 
is far more than it first seems. If we were to receive a PR, we'd have 
to basically throw it away and write it how we want it to be implemented.

We are grateful for community involvement, bug reports, & feature requests. We do
not wish to come off as anything but welcoming, however, we've
made the decision to keep this project closed to contributions for 
the long term viability of the project.

