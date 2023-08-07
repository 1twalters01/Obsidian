1)  Part 1 - The Rust Programming Language
	1) Getting started
		1) [[1) Installation]]
		2) [[2) Hello world - Rust]]
		3) [[3) Cargo]]
	2) Example
	3) Common Programming Concepts
		1) [[1) Variables and Mutability]]
		2) [[2) Data Types]]
		3) [[3) Functions]]
		4) [[4) Comments]]
		5) [[5) Control Flow]]
	4) Understanding Ownership
		1) [[1) What is Ownership]]
		2) [[2) References and Borrowing]]
		3) [[3) The Slice Type]]
	5) Using Structs to Structure Related Data
		1) [[1) Defining and Instantiating Structs]]
		2) [[2) An Example Program Using Structs]]
		3) [[3) Method Syntax]]
	6) Enums and Pattern Matching
		1) [[1) Defining an Enum]]
		2) [[2) The match Control Flow Construct]]
		3) [[3) Concise Control Flow with if let]]
	7) Managing Growing Projects with Packages, Crates, and Modules
		1) [[1) Managing growing Projects]]
		2) [[2) Defining Modules to Control Scope and Privacy]]
		3) [[3) Paths for Referring to an Item in the Module Tree]]
		4) [[4) Bringing Paths Into Scope with the use Keyword]]
		5) [[5) Separating Modules into Different Files]]
	8) Common Collections
		1) [[Storing Lists of Values with Vectors]]
		2) [[Storing UTF-8 Encoded Text with Strings]]
		3) [[Storing Keys with Associated Values in Hash Maps]]
	9) Error Handling
		1) [[Unrecoverable Errors with panic!]]
		2) [[Recoverable Errors with Result]]
		3) [[To panic! or Not to panic!]]
	10) Generic Types, Traits, and Lifetimes
		1) [[Generic Data Types]]
		2) [[Traits: Defining Shared Behaviour]]
		3) [[Validating References with Lifetimes]]
	11) Writing Automated Tests
		1) [[How to Write Tests]]
		2) [[Controlling How Tests are Run]]
		3) [[Test Organisation]]
	12) An I/O Project: Building a Command Line Program
		1) [[Accepting Command Line Arguments]]
		2) [[Reading a File]]
		3) [[Refactoring to Improve Modularity and Error Handling]]
		4) [[Develop the Library's Functionality with Test Driven Development]]
		5) [[Working with Environment Variables]]
		6) [[Writing Error Messages to Standard Error Instead of Standard Output]]
	13) Functional Language Features: Iterators and Closures
		1) [[Closures: Anonymous Functions that Capture Their Environment]]
		2) [[Processing a Series of Items with Iterators]]
		3) [[Improving Our I.O Project]]
		4) [[Comparing Performance: Loops vs. Iterators]]
	14) More about Cargo and Crates.io
		1) [[Customising Builds with Release Profiles]]
		2) [[Publishing a Crate to Crates.io]]
		3) [[Cargo Workspaces]]
		4) [[Installing Binaries from Crates.io with cargo install]]
		5) [[Extending Cargo with Custom commands]]
	15) Smart Pointers
		1) [[Using Box<T> to Point to Data on the Heap]]
		2) [[Treating Smart Pointers Like Regular References with the Deref Trait]]
		3) [[Running Code on Cleanup with the Drop Trait]]
		4) [[Rc<T>, the Reference Counted Smart Pointer]]
		5) [[RefCell<T> and the Interior Mutability Pattern]]
		6) [[Reference Cycles Can Leak Memory]]
	16) Fearless Concurrency
		1) [[Using Threads to Run Code Simultaneously]]
		2) [[Using Message Passing to Transfer Data Between Threads]]
		3) [[Shared-State Concurrency]]
		4) [[Extensible Concurrency with the Sync and Send Traits]]
	17) Object Oriented Programming Features of Rust
		1) [[Characteristics of Object-Oriented Languages]]
		2) [[Using Trait Objects That Allow for Values of Different Types]]
		3) [[Implementing an Object-Oriented Design Pattern]]
	18) Patterns and Matching
		1) [[All the places patterns can be ]]
		2) [[Refutability: Whether a Pattern Might Fail to Match]]
		3) [[Pattern Syntax]]
	19) Advanced Features
		1) [[Unsafe Rust]]
		2) [[Advanced Traits]]
		3) [[Advanced Types]]
		4) [[Advanced Types]]
		5) [[Advanced Functions and Closures]]
		6) [[6) Macros]]
	20) Final Project: Building a Multithreaded Web Server
		1) [[Building a Single-Threaded Web Server]]
		2) [[Turning Our Single-Threaded Server into a Multithreaded Server]]
		3) [[Graceful Shutdown and Cleanup]]
	21) Appendix
		1) A - Keywords
		2) B - Operators and Symbols
		3) C - Derivable Traits
		4) D - Useful Development Tools
		5) E - Editions
		6) F - Translations of the Book
		7) G - How Rust is Made and "Nightly Rust"
2) Part 2 - Rust and WebAssembly
	1) Introduction
	2) Background and Concepts
		1) What is WebAssembly?
		2) Why Rust and WebAssembly?
	3) Setup
		1) Hello World - Rust/WASM