How to build the JOGL OpenGL binding for Java

The following are the supported build environments and platforms for JOGL. All require the ANTLR parser generator and the Ant build system version 1.6 or later to be installed.

Solaris/SPARC and Solaris/x86, 32- and 64-bit
Solaris 8 or later
Sun ONE Studio 8 Compiler Collection or later
Sun OpenGL for Solaris (or Mesa for Solaris 9 x86)
Sun JDK 1.4.2 or later
Linux/x86, 32- and 64-bit
Red Hat Linux 7.3 or later
GCC
Sun JDK 1.4.2 or later
Macintosh PowerPC or Intel
Mac OS X 10.3 (note: will not work with earlier releases)
GCC
Java 1.4.2 or later for Mac OS X
Windows/x86 (32-bit currently, but 64-bit is known to work)
Windows 2000 or later
Microsoft Visual C++ 6.0 or later, or MinGW compilers
Sun JDK 1.4.2 or later
Additional platforms such as FreeBSD and HP/UX are handled by the build system, but are not officially supported.

Here are the steps that are required in order to build the JOGL OpenGL binding from a fresh copy of the source distribution, which can be obtained either from GIT or an archived build.

JOGL depends on our GlueGen project, which can be optained either from GIT or an archived build.

Install the JDK: 
the JOGL build requires JDK 1.4.2 or later. On AMD64 platforms such as Linux/AMD64, Solaris/AMD64 and Windows/AMD64, the build requires the Sun JDK 5.0 or later, as the 1.4.2 releases did not have an AMD64-specific JVM. On these platforms, it is also currently necessary to have e.g. bin/amd64/java in your PATH, although from a technical standpoint this could be worked around in the build process.
Install Ant: 
Download and unpack the latest version of Ant from http://ant.apache.org and add the bin/ subdirectory to your PATH environment variable.
Install ANTLR: 
Download and unpack the latest version of ANTLR from http://www.antlr.org. Only the jar file is needed.
Unset your CLASSPATH environment variable: 
The Ant build requires that the JOGL jars not be visible on the classpath. On Unix, type unsetenv CLASSPATH into a csh or tcsh shell, or unset CLASSPATH into a Bourne shell. On Windows, type set CLASSPATH= into a command prompt.
Check out the GlueGen source tree: 
JOGL relies on the GlueGen project to autogenerate most of the Java and JNI code for the OpenGL interface. The jogl/ and gluegen/ workspaces must be side-by-side in order for JOGL to build properly.
Copy and edit gluegen.properties: 
Copy make/gluegen.properties from the GlueGen workspace into your home directory (pointed to by the Java system property user.home). 
Edit the copy to change the location of the ANTLR jar file (typically $HOME/antlr-2.7.2/antlr.jar).
Copy and edit jogl.properties: 
Copy make/jogl.properties into your home directory (pointed to by the Java system property user.home). 
Edit the copy to change any settings that are necessary, in particular the setting of win32.c.compiler on Windows platforms (one of "vc6", "vc7", "vc8", "vc8_x64", or "mingw"). Note that on Windows 64-bit platforms the Professional edition of the Microsoft compilers is required.
The Windows build requires one of Microsoft Visual C++ 6, 7 (Visual Studio .NET), 8 (Microsoft Visual C++ 2005) or the free MinGW (http://www.mingw.org/) compilers to be installed. Choose the appropriate setting of win32.c.compiler for the compiler being used. The C compiler executable (cl.exe, gcc.exe) must be in your PATH; see below.
When building with VC6, VC7, or VC8, you must first run the vcvars32.bat environment variable setup script from the appropriate version of Visual C++. The Windows C compiler you choose in jogl.properties (i.e., win32.c.compiler=vc6) must match the version of the compiler from which you executed vcvars32.bat. No error checking is done on the compiler version used, so please be careful.
Build the source tree: 
Open a command shell in the "make" directory of the source tree and type "ant".
An experimental binding to the high-level Cg language by NVidia corporation can be generated by specifying -Djogl.cg=1 to ant; e.g. ant -Djogl.cg=1. The Cg binding has been tested on Windows, Linux, and Mac OS X.
Add JOGL and the GlueGen runtime to your CLASSPATH: 
To be able to use JOGL once built, you must add the build process' resulting jogl.jar (.../jogl/build/jogl.jar) and gluegen-rt.jar (.../gluegen/build/gluegen-rt.jar) to your CLASSPATH environment variable.
Add JOGL and the GlueGen runtime to your PATH, LD_LIBRARY_PATH, or DYLD_LIBRARY_PATH: 
To be able to use JOGL once built, you must also add the build process's JNI code library directories (.../jogl/build/obj and .../gluegen/build/obj) to your PATH (on Windows), LD_LIBRARY_PATH (on most Unix platforms), or DYLD_LIBRARY_PATH (on Mac OS X) environment variable.
Test if everything's working: 
To test if everything went well, you should check out the source code for the jogl-demos project (available at http://download.java.net/media/jogl/demos/www/), build the demos using the supplied instructions, and run the Gears demo ("java demos.gears.Gears").
Build Javadoc: 
"ant javadoc" will produce the end-user documentation for JOGL along with some auxiliary utility packages. The developers' documentation, including that for the GlueGen tool, can be generated for your current platform using one of the following commands: "ant javadoc.dev.win32", "ant javadoc.dev.x11", or "ant javadoc.dev.macosx". (The javadoc for the Cg binding can be built by inserting -Djogl.cg=1 into the command line as above.)
Note that there are a lot of warnings produced by ANTLR about the C grammar and our modifications to some of the signatures of the productions; the C grammar warnings have been documented by the author of the grammar as having been investigated completely and harmless, and the warnings about our modifications are also harmless.
Common build problems

Your CLASSPATH environment variable appears to be set (some JOGL classes are currently visible to the build.), and $CLASSPATH isn't set.
An older version of JOGL was installed into the extension directory of the JDK you're using to build the current JOGL. On Windows and Linux, delete any ANTLR jars from jre/lib/ext, and on Mac OS X, delete them from /Library/Java/Extensions. It is generally not a good idea to drop JOGL directly into the extensions directory, as this can interfere with upgrades via Java Web Start.
CharScanner; panic: ClassNotFoundException: com.sun.gluegen.cgram.CToken
This occurs because ANTLR was dropped into the Extensions directory of the JRE/JDK. On Windows and Linux, delete any ANTLR jars from jre/lib/ext, and on Mac OS X, delete them from /Library/Java/Extensions. Use the antlr.jar property in the build.xml to point to a JRE-external location of this jar file.
- Christopher Kline and Kenneth Russell, June 2003 (revised November 2006)
