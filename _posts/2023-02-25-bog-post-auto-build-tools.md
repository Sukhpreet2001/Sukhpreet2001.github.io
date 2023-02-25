# Introducing Auto Build Tools (Gradle)


This  blog is going to be on introduction of auto build tools and trying to understand using gradle.


## Automated Build Tool

Automated Build Tool is a software that compiles the source code to machine code.

Automation tools are used to automate the whole process of software build creation and the other related processes like packaging binary code and running the automated tests.
Build Automation Software reduce manual labor and validate the build consistency. It offers several benefits as well. However, there are some challenges for these tools i.e. long builds, a large volume of builds, and complex builds.

Build tools aid in improving performance, improving development productivity, or automating tasks.

 ### Build Tools can assist with:

* combining JavaScript modules and CSS into bundled files for production
* minifying files for improved performance
* running unit tests with one command
* automatically previewing changes to your application
--
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

--

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
