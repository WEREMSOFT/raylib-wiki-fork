### About Eclipse IDE for C/C++

**Eclipse** is the most widely used **IDE** for **Java** and it is famous for its **Java Integrated Development Environment (IDE)**, but there is also a cool version of the **IDE** which supports **C/C++**, for more info:

https://www.eclipse.org/downloads/packages/release/2018-12/r/eclipse-ide-cc-developers

In this guide we will cover the various installation steps.

### Configuring mingw-w64 on Windows

In order to work with **raylib** and **Eclipse** you need to download the **Mingw-w64 Installer** and follow the steps in the wizard.

When prompted choose the following settings:
* **Version:** 7.2.0
* **Architecture:** x86_64
* **Threads:** win32

Then proceed with the installation.

Once it is finished ensure that there is the path where your **MinGW-w64** has been installed to e.g., **C:\mingw\mingw64\bin** in your **PATH** environment variable (if there is not just add it manually).

### Building raylib

You can follow one of this guides for building **raylib** on your operative system:

* https://github.com/raysan5/raylib/wiki/Working-on-Windows
* https://github.com/raysan5/raylib/wiki/Working-on-macOS
* https://github.com/raysan5/raylib/wiki/Working-on-GNU-Linux

### Creating a new C/C++ Project

* Run **Eclipse IDE for C/C++**.
* From the main menu choose **File > New > C/C++ Project**.
* Select **C++ Managed Build** and **Next >**.
* Write the **Project name** then select **Hello World C++ Project** and **MinGW CC** under **Toolchains**.
* Click on **Finish** to create the new project.

Now you can replace the contents of the main **.cpp** file with some basic examples from the following link:

https://www.raylib.com/examples.html

### Configuring the Project for raylib

* From the main menu choose **Project > Properties**.
* Now go to **C/C++ Build > Settings > Tool Settings** tab.
* Under **GCC C++ Compiler > Includes > Include paths (-I)** add the path to your **raylib/src** folder.
* Under **MinGW C++ Linker > Libraries > Libraries (-l)** add **raylib** and **gdi32** libs.
* Under **MinGW C++ Linker > Libraries > Library search path (-L)** add the path to the **raylib** static library which you previously builded.

### Build and Run!

* Under the tab **Project Explorer** (you find on the left side of the screen) right click on your project.
* Select **Build Project** from the popup menu for building your project.
* Finally right click again on your project and then select **Run As > Local C/C++ Application**.

Now you should be ready to go with **Eclipse**!

Enjoy! :)