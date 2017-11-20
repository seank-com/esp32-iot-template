# ESP32 IoT Template

A model of the right way to structure an ESP32 based embedded project.

**DO NOT CLONE OR FORK THIS REPO** - This is meant to be a model reference so
you can see how to setup your own project. If you follow the steps below your
project should look like this one. If not, go back through the instructions and
find what you missed or open an
[issue](https://github.com/seank-com/esp32-iot-template/issues) so I can fix
the instructions.

## Setup Development tools

1. Following the docs in the
  [Espressif README](https://github.com/espressif/esp-idf) pick your **Setup
  Guide** and follow the steps to setup prerequisite tools, download and untar
  the tool chain. Stoping before you update your path or clone the esp-idf repo.

  **Discussion:** *If you read the docs for setting up your
  [build system](https://esp-idf.readthedocs.io/en/v1.0/build_system.html). You
  will see that Espressif recommends you keep your esp-idf seperate from your
  project. In embedded development, this is not ideal. Each project should be
  treated in isolation. While a new embedded project might use the latest bits,
  a shipped project should update much more slowly. So we will instead be using
  submodules*

2. If you don't already have a Serial USB driver installed, you may wish to
  install silicon lab's driver using brew (prolific's didn't work for me)

  ```bash
  $ brew cask install silicon-labs-vcp-driver
  ```
3. First lets create a new project on github and clone it locally.
  ```bash
  $ git clone _your_github_project_repo_link_here_ myproject
  $ cd myproject/
  ```
4. Add esp-idf as a submodule

  **Note:** *In the future, you may end up needing to make a contribution in the
  esp-idf project, to fix a bug in your embedded application. At that time you
  should fork the esp-idf repo and update your project to use your fork as a
  submodule instead so you can benefit from your changes until your PR gets
  accepted.*

  ```bash
  $ git submodule add https://github.com/espressif/esp-idf.git esp-idf
  # git submodule add --recursive doesn't exist, so do this
  $ git submodule update --init --recursive
  ```

5. Add the azure-iot component as a submodule

  **Note:** *The note in Step 3 applies here as well. You might think you could
  also add submodules directly into the esp-idf/components folder (assuming you
  forked esp-idf first) Unfortunately, you would then need to manage submodule
  versions in two different repos and this can become unwieldy very quickly.*

  ```bash
  $ mkdir -p components
  $ cd components/
  $ git submodule add https://github.com/Azure/azure-iot-pal-esp32.git azure-iot
  $ git submodule update --init --recursive
  $ cd ..
  ```

6. Create a devenv configuration file that contains the following:  

  ```bash
  # update path
  export PATH=$PATH:~/esp/xtense-esp32-elf/bin
  # locate idf and extra components
  export PROJECT_DIR=_path_to_your_project_
  export IDF_PATH=$PROJECT_DIR/esp-idf
  export EXTRA_COMPONENT_DIRS=$PROJECT_DIR/components
  ```

7. Create a folder next to esp-idf and components called main and put your code
  there

8. Configure and build

  ```bash
  # setup your environment
  $ source devenv
  # You only need to do this step the first time. Be sure to navigate to
  # Example configuration and add your WiFi SSID, Password and IoTHub
  # connection string and save the sdkconfig file.
  $ make menuconfig
  # and build
  $ make
  ```

9. Instructions for other collaborators

  Once you have your project committed and pushed to your repository in the
  cloud, be sure to tell other developer to use the ```--rescursive``` parameter
  when cloning the repo. If they forget, they'll need to use the following
  command to fix their mistake.

  ```bash
  $ git submodule update --init --recursive
  ```
