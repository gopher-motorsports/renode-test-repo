[![Actions Status](https://github.com/gopher-motorsports/renode-test-repo/workflows/Build/badge.svg)](https://github.com/gopher-motorsports/renode-test-repo/actions?query=workflow%3ABuild)
[![Actions Status](https://github.com/gopher-motorsports/renode-test-repo/workflows/SIL%20Tests/badge.svg)](https://github.com/gopher-motorsports/renode-test-repo/actions?query=workflow%3ASIL%20Tests)

# renode-test-repo
This is a skeleton repo to act as a proof-of concept for a dockerized Robot SIL test system running on top of the Renode emulator.

## What is this repo?
As you may be able to tell, this repository contains an STM32CubeIDE project, as well as a `validation` directory. This structure intends to allow developers to work on code like normal,
but to allow them to quickly execute a battery of SIL tests using the Renode emulator and the robot testing framework. The rests are executed in a docker container, allowing for
consistent test environments between developers, as well as providing an easy way to execute tests on a CI runner

## Prerequisites
As far as I know, all you should need to work with this repo is:
- The GNU ARM embedded toolchain
- Docker (if using windows, preferably Docker Desktop. Docker toolbox may work but has not been tested)
- (Optional) STM32CubeIDE

## How do I interact with this repo?
Simple: write code! The `.project` file can be opened in STM32CubeIDE if you want an IDE, otherwise, you can edit the files with a text editor and use `make` to build the binaries.
When you want to test your code locally, execute the following steps:
1. Build the debug configuration. This can be done by executing `make all` in the `Debug` folder, or by using the UI in STM32CubeIDE
2. `cd` into the `validation` directory and run the following command: `docker-compose up`
(This will pull and build the renode container, mount some local directories, launch the container and execute all of the tests in `tests.yaml`)
3. View the results in the `results` directory

## How do I execute tests on a CI runner?
Just commit and push! A webhook will dispatch two GitHub actions jobs (build and test) that will first attempt to build the debug configuration, then execute the SIL tests using the same docker container as you do locally. This, as mentioned previously, allows for a consistent test environment across all machines.

# DO NOT COMMIT CHANGES TO THE MAKEFILE!
It's totally fine to change it locally (STM32CubeIDE does this when you build), but because CubeMX defaults to using absolute paths, it will break the build process on 
GitHub Actions if changes are pushed.
