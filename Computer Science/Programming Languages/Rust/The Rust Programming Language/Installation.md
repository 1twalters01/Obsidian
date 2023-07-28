# Installation
## Windows
1) Go to https://www.rust-lang.org/tools/install
2) Follow the instructions
   * You will need the MSVC build tools for Visual Studio 2013 or later
   * To acquire the build tools, you'll need to install Visual Studio 2022
     * When asked which workloads to install, include:
       * "Desktop Development with C++"
       * The Windows 10 or 11 SDK
       * The English language pack component, along with any language pack of your choosing
3) To check whether you have Rust installed correctly, open a shell and enter the line: "rustc --version"
   * You should see the version number, commit hash, and commit date for the latest stable release
   * If you don't, then check that Rust is in your %PATH% system variable

# Updating and Uninstalling
To update Rust, run the following in your shell: "rustup update"
To uninstall Rust, run the following in your shell: "uninstall"

# Legal Documentation
To open the local documentation in your browser run "rustup doc"
