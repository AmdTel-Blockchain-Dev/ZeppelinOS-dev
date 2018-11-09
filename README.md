## Dev Env
This project will require Node.js and npm. If your comfortable with this env you can probably skip most of thes steps.  I ran into some permissions problems and found the simplest solution was not to use a system installed node but rather develp on a user installed node version.

Prefered Method: Reinstall npm with a Node version manager (recommended)

fyi: [How to Prevent Permissions Errors when using NPM](https://docs.npmjs.com/getting-started/fixing-npm-permissions#how-to-prevent-permissions-errors)

Run as a normal user:

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```
(restart your terminal if nvm is not available on the command line.)

Install the latest node version or choose which version you would like to develop with (e.g nvm install 8.0.0).
```
nvm install node
```
Check and install latest npm version:
```
nvm install-latest-npm
```
Then install the ZeppelinOS package
```
npm install --global zos
```
### Setting up your project
Brief summery of [Deploying your first project](https://docs.zeppelinos.org/docs/deploying.html) from ZeppelinOS Documents
```
mkdir my-project
cd my-project
npm init
zos init my-project
```
