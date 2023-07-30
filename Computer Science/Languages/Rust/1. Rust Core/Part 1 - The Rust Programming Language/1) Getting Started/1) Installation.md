# Installation
## Linux or macOS
1) Open a terminal and enter the following command:

```terminal
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

3) If the install is successful, the following line will appear: `Rust is installed now. Great!`
4) You will also need a linker - a program that Rust uses to join its compiled outputs into one file.
	* If you get linker errors, you should install a C compiler, which will typically include a linker. A C compiler is also useful as some common Rust packages depend on C code and will need a C compiler
	* On macOS, you can get a C compiler by running: `xcode-select --install`
	* Linux users should install GCC or Clang, according to their distribution's documentation.

## Windows
1) Go to https://www.rust-lang.org/tools/install
2) Follow the instructions
   * You will need the MSVC build tools for Visual Studio 2013 or later
   * To acquire the build tools, you'll need to install Visual Studio 2022
     * When asked which workloads to install, include:
       * `Desktop Development with C++``
       * The Windows 10 or 11 SDK
       * The English language pack component, along with any language pack of your choosing

# Troubleshooting
1) To check whether you have Rust installed correctly, open a shell and enter the line: `rustc --version`
   * You should see the version number, commit hash, and commit date for the latest stable release
   * If you don't, then check that Rust is in your ``%PATH%`` system variable

# Updating and Uninstalling
To update Rust, run the following in your shell: `rustup update`
To uninstall Rust, run the following in your shell: `rustup self uninstall`

# Legal Documentation
To open the local documentation in your browser run `rustup doc`
