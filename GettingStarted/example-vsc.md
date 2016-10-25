# Example Hello World

## Your First `Electron Dotnet` Application

This document will take you through creating your first `Electron Dotnet` application for VS Code ("Hello World") and will explain the basic `Electron` integration points to add C# code and Plugins to your `Electron` application.

In this walkthrough, you'll add a C# and a new Plugin to an Electron application which will display a simple "Hello" message from them both. Later in the walkthrough, you'll see how we can debug the different `Electron` processes, Main and Renderer.

## Prerequisites
You need [node.js](https://nodejs.org/en/) installed and available in your $PATH.
   * Plugins require [Mono](http://www.mono-project.com/download/) installed and available in your $PATH.
      
      > :bulb: Windows will need both the x86 and x64 bit versions installed and [available in your $PATH](https://github.com/xamarin/WebSharp/tree/master/electron-dotnet#setting-mono-path).
   * `electron-dotnet` needs to be built.  The easiest way is to use the provided `make` files available in the WebSharp base directory.
     ``` bash
     # Windows Visual Studio 2015 Command Line Prompt 
     nmake /f Makefile.win buildRelease
     ```

     ``` bash
     # Mac OSX terminal with XCode tools available for build.
     make setup  # only needs to be run the first time
     make build
     ```
     
## Getting Started      
  * Install [.NET Core.](https://www.microsoft.com/net/core)
  * Install the [C# extension from the VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## Generate a `Electron Dotnet` Application
The simplest way to add a new `Electron DotNet` application for VS Code consumption is through adding a project. A project will have all the boot strap files available to run the `Electron` application.  For more details you can look at the [Electron Quick Start](https://github.com/electron/electron-quick-start).

We have written a Yeoman generator to help get you started. Install Yeoman and the Yeoman Electron Dotnet generator that provides different templates for creating new applications:

``` bash
npm install -g yo path-to-WebSharp-directory\Tools\generator-electron-dotnet
yo electron-dotnet
```
You will be presented with three different project types. For this example, we will pick `Both C# and Plugin`.  

![The electron-dotnet generator](./screenshots/yogen.PNG)

Once the type of application project is selected the generator will present you with a series of questions so that the generator can generate the application for you.  Fill in the answers as shown in the following screen shot.

![The electron-dotnet generator questions](./screenshots/yogenask.PNG)

Hit enter to start generating the application structure.

![The electron-dotnet generator install](./screenshots/yogeninstall.PNG)

This will install the dependencies automatically and when done you will have the application generated.

![The electron-dotnet generator finish](./screenshots/yogenfinish.PNG)

## The Structure of an application
After running, the generated application should have the following structure:

```
.
|--- .eslintrc.json
|--- .gitignore
|--- .vscode                           // VS Code integration
     |--- launch.json
     |--- settings.json
|--- .vscodeignore
|--- electron-dotnet-quickstart.md
|--- index.html                       // Html to be displayed in the app window
|--- jsconfig.json
|--- main.js                          // Defines the electron main process
|--- node_modules
     |--- All the node files used to run the electron application
|--- package.json                     // Various project metadata
|--- README.md
|--- renderer.js                      // Required in index.html and executed in the renderer process for that window 
|--- src                              // sources
     |--- hello-world.cs              // PepperPlugin implementation
     |--- hello-world.js              // C# code implementation
     |--- project.json                // Defines compilation infomration 

```

Let's go through the purpose of some of these files and explain what they do:

### > Needs more information

## Compiling plugin code
Before running our application we will need to compile our plugin code ```hello-world.cs```.

The compile target uses [Dotnet Core](https://www.microsoft.com/net/core) with one of the dependencies of the Plugin API `Xamarin.PepperSharp` delivered as a NuGet package.

``` bash
# Windows
cd src
HelloWorld\src> dotnet restore -s path-to-WebSharp\electron-dotnet\tools\build\nuget
HelloWorld\src> dotnet build
HelloWorld\src> dotnet publish
cd ..
```

``` bash
# Mac OSX
cd src
HelloWorld\src$ dotnet restore -s path-to-WebSharp/electron-dotnet/tools/build/nuget
HelloWorld\src$ dotnet build
HelloWorld\src$ dotnet publish
cd ..
```

### What do the commands above do?

* Resolve the build assets by typing `dotnet restore`.
  * Running `restore` pulls down the required packages declared in the project.json file.
  * You'll see a new project.lock.json file in your project folder.
  * This file contains information about your project's dependencies to make subsequent restores quicker.
  * The `-s path-to-WebSharp/electron-dotnet/tools/build/nuget` in the `restore` is the nuget source where the `Xamarin.PepperSharp.xxx.nupkg` can be found.
     * On Windows if a Local Package source is setup then the source will be search so you will not need to provide this parameter.
     * On Mac it seems that the `restore` does not work for Local Package sources right now.  Your mileage may vary but this is the surefire way to get the dependencies restored correctly.
  * The `Xamarin.PepperSharp.xxxx.nupkg` dependency is built during the Electron DotNet build process.
* Build the source `hello-world.cs` implementation by typing `dotnet build`.
  * The `build` command will compile the source file based on the definition found in the `project.json`
* Make the assemblies available for use by typing `dotnet publish`.
  * This will copy the implementation as well as the `Xamarin.PepperSharp.dll` from the nuget package available to be loaded.
  * The `PepperPlugin` does not support loading from a nuget package so the `publish` will make all the dependencies available in one place so they can be loaded.

## Running the application

To run the application we will need to install 'electron-dotnet' module which provides all of the ```Node.js``` implemenation for running within ```Electron```.

> :bulb: This project dependency will be automatic in the future once the project workflow has been defined and will be installed with the template in the future.

But right now we will have to do this install manually from the command line.

``` bash
# Windows
HelloWorld> npm install path-to-WebSharp\electron-dotnet   
HelloWorld> npm start 
```

> :bulb: Windows users need to make sure that mono is [available in their $PATH](https://github.com/xamarin/WebSharp/tree/master/electron-dotnet#setting-mono-path).  If not then a runtime error is generated.


``` bash
# Mac OSX
HelloWorld$ npm install path-to-WebSharp/electron-dotnet   
HelloWorld$ npm start 
```






