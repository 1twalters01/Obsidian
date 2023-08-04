# NVM
NVM, or Node Version Manager, is a tool for managing node versions. We will be using this as different projects may be using different versions of node. As such, you may get errors if you use a different version.

## Installation
### Linux or macOS
1) Write the following into your terminal:
	* `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash`
2) If this didn't automatically add nvm configuration, you can add it yourself to your profile file:
```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```
4) With your profile configuration updated, now you will reload the configuration for your terminal to use: `source ~/.bashrc`
5) Check that it has installed using the command `nvm --version`
### Windows
1) Go to [nvm-windows](https://github.com/coreybutler/nvm-windows/releases), scroll down slightly and click on download now.
2) Click on `nvm-setup.exe`
3) Complete the installation wizard
4) Check that it has installed using the command `nvm --version`

## Usage
* Install the latest version of node using `nvm install latest`.
* Install a specific version of node using `nvm install vX.Y.Z`
* Make a version your default by running `nvm alias default vX.Y.Z`
* Run the following in your terminal to use a specific version of node: `nvm use vA.B.C`