# Installation
## Linux
Python is installed on every Linux system, however it may be outdated.
Type the following into the command line:
1) `sudo apt-get update`
2) `sudo apt-get install python3.10 python3-pip` (or whatever is the current version)

## macOS
1) Install homebrew:
	* Open a browser and navigate to `http://brew.sh/`
	* You should see a command for installing Homebrew near the top of the page under the tile “Install Homebrew.”
	* Enter this into a terminal window and complete all the steps 
	* Open a terminal window and enter `brew update && brew upgrade`
2) Run the command `brew install python3` in a terminal

## Windows
1) Go to https://www.python.org/downloads/
2) Find a stable python release
3) Click the appropriate link for your system to download the executable file
4) After the installer is downloaded, double-click the .exe file
5) Select the Install launcher for all users checkbox
6) Select the Add python.exe to PATH checkbox

7) The Optional Features include common tools and resources for Python
	* Documentation: recommended
	* pip: recommended
	* tcl/tk and IDLE: not needed
	* Python test suite: recommended
	* py launcher and for all users: recommended

9) For Advanced Options
	* Install for all users: recommended
	* Associate files with Python: recommended
	* Create shortcuts for installed applications: recommended
	* Add Python to environment variables: recommended
	* Precompile standard library: recommended
	* Download debugging symbols and Download debug binaries: not needed
		* You will need the MSVC build tools for Visual Studio 2013 or later for this
		* To acquire the build tools, you'll need to install Visual Studio 2022
		* When asked which workloads to install, include:
		* `Desktop Development with C++``
		* The Windows 10 or 11 SDK
		* The English language pack component, along with any language pack of your choosing

10) Carry on with the installation

# Troubleshooting
1) To check whether you have Python installed correctly, open a shell and enter the line: `python --version`
	* Check the Python 2 version with `python2 --version` and the Python 3 version with `python3 --version`
	* You should see the version number, commit hash, and commit date for the latest stable release
	* If you don't, then check that Python is in your ``%PATH%`` system variable
