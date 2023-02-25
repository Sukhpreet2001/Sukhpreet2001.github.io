# Introducing Auto Build Tools (Gradle)


This  blog is going to be on introduction of auto build tools and trying to understand using gradle.


## Automated Build Tool

Automated Build Tool is a software that compiles the source code to machine code.

Automation tools are used to automate the whole process of software build creation and the other related processes like packaging binary code and running the automated tests.
Build Automation Software reduce manual labor and validate the build consistency. It offers several benefits as well. However, there are some challenges for these tools i.e. long builds, a large volume of builds, and complex builds.

Build tools aid in improving performance, improving development productivity, or automating tasks.



 **Build Tools can assist with: **

* combining JavaScript modules and CSS into bundled files for production
* minifying files for improved performance
* running unit tests with one command
* automatically previewing changes to your application





### List Of The Top Build Automation Tools

Comparison of the Best Automated Build Deployment Software
![image](https://user-images.githubusercontent.com/79241977/221318952-aa2e723b-b65d-43de-922b-7fbdc9e1f8cb.png)

So , Now we are going to discuss about Gradle and how one can use it to build/compile **C/C++ Projects** with multiple files

## What is Gradle?

Gradle is an open-source build automation tool flexible enough to build almost any type of software. Gradle makes few assumptions about what you’re trying to build or how to build it. This makes Gradle particularly flexible.

### Design
Gradle bases its design on the following fundamentals:

### High performance
Gradle avoids unnecessary work by only running tasks that need to do work because inputs or outputs have changed. Gradle uses various caches to reuse outputs from previous builds. With a shared build cache, you can even reuse outputs from other machines.

### JVM foundation
Gradle runs on the JVM. This is a bonus for users familiar with Java, since build logic can use the standard Java APIs. It also makes it easy to run Gradle on different platforms.

### Conventions
Gradle makes common types of projects easy to build through conventions. Plugins set sensible defaults to keep build scripts minimal. But these conventions don’t limit you: you can configure settings, add your own tasks, and make many other customizations in your builds.

### Extensibility
Most builds have special requirements that require custom build logic. You can readily extend Gradle to provide your own build logic with custom tasks and plugins. See Android builds for an example: they add many new build concepts such as flavors and build types.

### IDE support
Several major IDEs provide interaction with Gradle builds, including Android Studio, IntelliJ IDEA, Eclipse, VSCode, and NetBeans. Gradle can also generate the solution files required to load a project into Visual Studio.

### Insight
Build Scan™ provides extensive information about a build that you can use to identify issues. You can use Build Scans to identify problems with a build’s performance and even share them for debugging help.

## Terminology
It’s helpful to know the following terminology before you dive into the details of Gradle.

### Projects
Projects are the things that Gradle builds. Projects contain a build script, which is a file located in the project’s root directory usually named build.gradle or build.gradle.kts. Builds scripts define tasks, dependencies, plugins, and other configuration for that project. A single build can contain one or more projects and each project can contain their own subprojects.

### Tasks
Tasks contain the logic for executing some work—​compiling code, running tests or deploying software. In most use cases, you’ll use existing tasks. Gradle provides tasks that implement many common build system needs, like the built-in Java Test task that can run tests. Plugins provide even more types of tasks.

Tasks themselves consist of:

**Actions**: pieces of work that do something, like copy files or compile source

**Inputs**: values, files and directories that the actions use or operate on

**Outputs**: files and directories that the actions modify or generate

### Plugins
Plugins allow you to introduce new concepts into a build beyond tasks, files and dependency configurations. For example, most language plugins add the concept of source sets to a build.

Plugins provide a means of reusing logic and configuration across multiple projects. With plugins, you can write a task once and use it in multiple builds. Or you can store common configuration, like logging, dependencies, and version management, in one place. This reduces duplication in build scripts. Appropriately modeling build processes with plugins can greatly improve ease of use and efficiency.

### Build Phases
Gradle evaluates and executes build scripts in three build phases of the Build Lifecycle:

### Initialization
Sets up the environment for the build and determine which projects will take part in it.

### Configuration
Constructs and configures the task graph for the build. Determines which tasks need to run and in which order, based on the task the user wants to run.

### Execution
Runs the tasks selected at the end of the configuration phase.

### Builds
A build is an execution of a collection of tasks in a Gradle project. You run a build via the command line interface (CLI) or an IDE by specifying task selectors. Gradle configures the build and selects the tasks to run. Gradle runs the smallest complete set of tasks based on the requested tasks and their dependencies.
Installation
If all you want to do is run an existing Gradle build, then you don’t need to install Gradle if the build has a Gradle Wrapper, identifiable via the gradlew and/or gradlew.bat files in the root of the build. You just need to make sure your system satisfies Gradle’s prerequisites.

Android Studio comes with a working installation of Gradle, so you don’t need to install Gradle separately in that case.

In order to create a new build or add a Wrapper to an existing build, you will need to install Gradle according to these instructions. Note that there may be other ways to install Gradle in addition to those described on that page, since it’s nearly impossible to keep track of all the package managers out there.



### Building/compiling C++ Pojects
The simplest build script for a C++ project applies the C++ application plugin or the C++ library plugin and optionally sets the project version:

 Example 1. Applying the C++ Plugin
```ruby
build.gradle 
plugins {
    id 'cpp-application' // or 'cpp-library'
}

version = '1.2.1'
```


By applying either of the C++ plugins, you get a whole host of features:

- ``compileDebugCpp`` and ``compileReleaseCpp`` tasks that compiles the C++ source files under src/main/cpp for the well-known debug and release build types, respectively.

- ``linkDebug`` and ``linkRelease`` tasks that link the compiled C++ object files into an executable for applications or shared library for libraries with shared linkage for the debug and release build types.

- ``createDebug`` and ``createRelease`` tasks that assemble the compiled C++ object files into a static library for libraries with static linkage for the debug and release build types.

### Declaring your source files

Gradle’s C++ support uses a ``ConfigurableFileCollection`` directly from the application or library script block to configure the set of sources to compile.

Libraries make a distinction between private (implementation details) and public (exported to consumer) headers.

You can also configure sources for each binary build for those cases where sources are compiled only on certain target machines.
![image](https://user-images.githubusercontent.com/79241977/221329169-48c68b86-dd07-4650-815b-f114e2bd532e.png)

Gradle provides support for consuming pre-built binaries from Maven repositories published by Gradle [1].

We will cover how to add dependencies between projects within a multi-build project.

Specifying dependencies for your C++ project requires two pieces of information:

- Identifying information for the dependency (project path, Maven GAV)

- What it’s needed for, e.g. compilation, linking, runtime or all of the above.

This information is specified in a ``dependencies {}`` block of the C++ application or library script block. For example, to tell Gradle that your project requires library common to compile and link your production code, you can use the following fragment:

Example 2. Declaring dependencies
```ruby
build.gradle
application {
    dependencies {
        implementation project(':common')
    }
}

```

The Gradle terminology for the three elements is as follows:

*Configuration* (ex: ``implementation``) - a named collection of dependencies, grouped together for a specific goal such as compiling or linking a module

*Project reference* (ex: ``project(':common')``) - the project referenced by the specified path

---

### Compiling and linking your code
Compiling both your code can be trivially easy if you follow the conventions:

Put your source code under the *src/main/cpp* directory

Declare your compile dependencies in the ``implementation`` configurations (see the previous section)

Run the ``assemble`` task

We recommend that you follow these conventions wherever possible, but you don’t have to.

### Supported tool chain
Gradle offers the ability to execute the same build using different tool chains. When you build a native binary, Gradle will attempt to locate a tool chain installed on your machine that can build the binary. Gradle select the first tool chain that can build for the target operating system and architecture. In the future, Gradle will consider source and ABI compatibility when selecting a tool chain.

Gradle has general support for the three major tool chains on major operating system: Clang [2], GCC [3] and Visual C++ [4] (Windows-only). GCC and Clang installed using Macports and Homebrew have been reported to work fine, but this isn’t tested continuously.

#### Windows
To build on Windows, install a compatible version of Visual Studio. The C++ plugins will discover the Visual Studio installations and select the latest version. There is no need to mess around with environment variables or batch scripts. This works fine from a Cygwin shell or the Windows command-line.

Alternatively, you can install Cygwin or MinGW with GCC. Clang is currently not supported.

#### macOS
To build on macOS, you should install Xcode. The C++ plugins will discover the Xcode installation using the system PATH.

The C++ plugins also work with GCC and Clang installed with Macports or Homebrew [5]. To use one of the Macports or Homebrew, you will need to add Macports/Homebrew to the system PATH.

#### Linux
To build on Linux, install a compatible version of GCC or Clang. The C++ plugins will discover GCC or Clang using the system PATH.

### Customizing file and directory locations
Imagine you have a legacy library project that uses an src directory for the production code and private headers and include directory for exported headers. The conventional directory structure won’t work, so you need to tell Gradle where to find the source and header files. You do that via the ``application`` or ``library`` script block.

Each component script block, as well as each binary, defines where it’s source code resides. You can override the convention values by using the following syntax:

Example 3. Setting C++ source set
```ruby
build.gradle
library {
    source.from file('src')
    privateHeaders.from file('src')
    publicHeaders.from file('include')
}
```
Now Gradle will only search directly in src for the source and private headers and in include for public headers.

### Changing compiler and linker options
Most of the compiler and linker options are accessible through the corresponding task, such as ``compileVariantCpp``, ``linkVariant`` and ``createVariant``. These tasks are of type CppCompile, LinkSharedLibrary and CreateStaticLibrary respectively. 

For example, if you want to change the warning level generated by the compiler for all variants, you can use this configuration:

Example 4. Setting C++ compiler options for all variants
```ruby
build.gradle
tasks.withType(CppCompile).configureEach {
    // Define a preprocessor macro for every binary
    macros.put("NDEBUG", null)

    // Define a compiler options
    compilerArgs.add '-W3'

    // Define toolchain-specific compiler options
    compilerArgs.addAll toolChain.map { toolChain ->
        if (toolChain in [ Gcc, Clang ]) {
            return ['-O2', '-fno-access-control']
        } else if (toolChain in VisualCpp) {
            return ['/Zi']
        }
        return []
    }
}
```
It’s also possible to find the instance for a specific variant through the ``BinaryCollection`` on the ``application`` or ``library`` script block:

Example 5. Setting C++ compiler options per variant
```ruby
build.gradle
application {
    binaries.configureEach(CppStaticLibrary) {
        // Define a preprocessor macro for every binary
        compileTask.get().macros.put("NDEBUG", null)

        // Define a compiler options
        compileTask.get().compilerArgs.add '-W3'

        // Define toolchain-specific compiler options
        if (toolChain in [ Gcc, Clang ]) {
            compileTask.get().compilerArgs.addAll(['-O2', '-fno-access-control'])
        } else if (toolChain in VisualCpp) {
            compileTask.get().compilerArgs.add('/Zi')
        }
    }
}
```
### Selecting target machines
By default, Gradle will attempt to create a C++ binary variant for the host operating system and architecture. It is possible to override this by specifying the set of TargetMachine on the application or library script block:

Example 6. Setting target machines
```ruby
build.gradle
application {
    targetMachines = [
        machines.linux.x86_64,
        machines.windows.x86, machines.windows.x86_64,
        machines.macOS.x86_64
    ]
}
```
### Packaging and publishing
How you package and potentially publish your C++ project varies greatly in the native world. Gradle comes with defaults, but custom packaging can be implemented without any issues.

- Executable files are published directly to Maven repositories.

- Shared and static library files are published directly to Maven repositories along with a zip of the public headers.

- For applications, Gradle also supports installing and running the executable with all of its shared library dependencies in a known location.

### Cleaning the build
The C++ Application and Library Plugins add a clean task to you project by using the base plugin. This task simply deletes everything in the ``$buildDir`` directory, hence why you should always put files generated by the build in there. The task is an instance of Delete and you can change what directory it deletes by setting its ``dir`` property
