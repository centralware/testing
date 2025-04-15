# Testing / Sandbox

* The key for this repo is temporary and will be deleted once testing is complete
* The idea here is to stress-test what we can get away with from Linux to Git in relation to setting up "empty" templates for incomplete extensions. There are tens of thousands of extensions, so this is necessary.
* The final repository will have a "complete" and "incomplete" section for extensions, kernels, toolchain objects, etc.  How exactly it gets organized is still up in the air.

* Each extension has a build script and may have one or more support files it depends on (such as configuration files, init launchers, patches, etc.)  mkbuild (in the support package) will create a container script which includes everything found in the extension so that when launched, all necessary files are available at build time.
* Each extension is derrived from a template which we try to follow for all packages being created thus allowing a standardization to be formed.
  * The first area is fields to be filled in that describe the extension (author, copyright, etc.)
  * The next area is dedicated to determining what the most recent STABLE version is available online, where applicable.  MOST extensions will have SOME form of versioning online.
  * Next, we need to determine any BUILD dependencies the extension has - and if any of them are not available, we need to compile dependencies first.
  * Finally, we get to the configuration and compilation areas and if successful, the package is contained in a SquashFS or GZip archive along with a copy of clean source code which is kept offline save for the most recent releases.  (We're considering building a DVD/BluRay jukebox to keep archives not only safe, but available as needed...  but that's quite a bit down the road.)
  * All existing TCL extensions and all new extensions must follow the same function naming convention to ensure the server network can accomplish each build successfully on each platform.
    
> [!WARNING]
> All build scripts are tested and checked for functions, fields and responses.  No build scripts are permitted to have output which is not explicitly requested.
> A testing system is available to ensure build scripts accomplish their goals in the necessary format needed by our server farm.

For example:
```
getVersion()
{
   # A function to scrape internet resources for the most recent STABLE version of this extension
}
```
This function is called by a special server which has internet access, but acts like a proxy as it has no access to the server farm. When it is told that we need to build a given package, its first job is to find out how to get this version information.  (Even if we are compiling a specific version older than this, it's still executed.)  Afterward, it is instructed to download the source package from the author's website (or GIT) if we do not already have a copy.  That's all this machine is used for, so it's not one of the Muscle Machines.

All patch files, configuration files, init scripts, etc. are expected to be packaged with the builder so along with the downloaded source file from above, assuming all of the BUILD DEPENDENCY extensions already exist and are loaded, compilation should go through without a hitch!  If successful, the person (or bot) who requested the extension to be built is notified with a time sensitive link to download the RAW extension.  If it failed, the author or maintainer of the extension is notified of the situation along with log files from the attempt.

Tiny Core Linux, for example, would then take the RAW extension and rename it according to their repository needs, attach .dep .info .md5 and .list files and upload it to their repository server for each hardware platform requested.  For RPM or DEB the RAW extension is extracted and repackaged as needed, again, adding informative details where necessary.  This concept should end up creating the world's largest one-stop repository with an automated back-end consisting of dozens of hardware devices which may eventually be tuned to handle cross-compilation where necessary.



For sake of speed and efficiency, we are going to start this project using as many publicly available resources, including those found in different distributions which may already be partially functional.  (Fully functional would be a dream come true!)
For these instances, we're going to have a ***convert*** directory which contains the seeds from different distributions to be used as a guide to create functioning Simple Linux Builders.