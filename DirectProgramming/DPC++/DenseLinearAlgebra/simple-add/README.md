# `Simple Add` Sample

The `Simple Add` sample demonstrates the simplest programming methods for using SYCL*-compliant buffers and Unified Shared Memory (USM).

For comprehensive instructions, see the [Intel&reg; oneAPI Programming
Guide](https://software.intel.com/en-us/oneapi-programming-guide) and search
based on relevant terms noted in the comments.

| Property            | Description 
|:---                 |:---
| What you will learn | How to use SYCL*-compliant extensions to offload computations using both buffers and USM.
| Time to complete    | 15 minutes

## Purpose

The `Simple Add` sample is a simple program that adds two large vectors of
integers and verifies the results. In this sample, you will see how to use 
the most basic code in C++ language that offloads computations to a GPU, which includes using USM and buffers.

The basic SYCL* implementations explained in the sample includes device selector,
USM, buffer, accessor, kernel, and command groups.

## Prerequisites

| Optimized for                     | Description
|:---                               |:---
| OS                                | Ubuntu* 18.04 <br> Windows* 10
| Hardware                          | Skylake with GEN9 or newer <br>Intel&reg; Programmable Acceleration Card with Intel&reg; Arria&reg; 10 GX FPGA
| Software                          | Intel&reg; oneAPI DPC++/C++ Compiler

### Known Issues

As of the oneAPI 2021.4 release, the argument for accessors was changed from `noinit` to
`no_init`. The change was derived from a change between the SYCL 2020 provisional spec and that of the 2020 Rev3 spec.

If you run the sample program and it fails, try one of the following actions:
- Update the Intel® oneAPI Base Toolkit to the latest version.
- Change the `no_init` argument  to `noinit`

## Key Implementation Details

This sample provides examples of both buffers and USM implementations for simple side-by-side comparison.

- USM requires an explicit wait for the asynchronous kernel's
computation to complete.

- Buffers, at the time they go out of scope, bring main
memory in sync with device memory implicitly. The explicit wait on the event is
not required as a result.

The program attempts first to run on an available GPU, and it will fall back to the system CPU if it does not detect a compatible GPU. If the program runs successfully, the name of the offload device and a success message is displayed.

> **Note**: Successfully building and running the sample proves your development environment is configured correctly.

## Setting Environment Variables
For working with the Command-Line Interface (CLI), you should configure the oneAPI toolkits using environment variables. Set up your CLI environment by
sourcing the `setvars` script every time you open a new terminal window. This practice ensures your compiler, libraries, and tools are ready for development.

> **Note**: If you have not already done so, set up your CLI
> environment by sourcing  the `setvars` script located in
> the root of your oneAPI installation.
>
> Linux*:
> - For system wide installations: `. /opt/intel/oneapi/setvars.sh`
> - For private installations: `. ~/intel/oneapi/setvars.sh`
> - For non-POSIX shells, like csh, use the commands similar to the following: `$ bash -c 'source <install-dir>/setvars.sh ; exec csh'`
>
> Windows*:
> - `C:\Program Files(x86)\Intel\oneAPI\setvars.bat`
> - For Windows PowerShell*, use the following command: `cmd.exe "/K" '"C:\Program Files (x86)\Intel\oneAPI\setvars.bat" && powershell'`
>
> For more information on configuring environment variables, see [Use the setvars Script with Linux* or MacOS*](https://www.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top/oneapi-development-environment-setup/use-the-setvars-script-with-linux-or-macos.html) or [Use the setvars Script with Windows*](https://www.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top/oneapi-development-environment-setup/use-the-setvars-script-with-windows.html).

You can use [Modulefiles scripts](https://www.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top/oneapi-development-environment-setup/use-modulefiles-with-linux.html) to set up your development environment. The modulefiles scripts work with all Linux shells.

## Include Files
Many oneAPI code samples, like this one, share common include files. The include files are installed locally and are located at `%ONEAPI_ROOT%\dev-utilities\latest\include` on your development system. You will need the `dpc_common.hpp` from that location to build these samples.

### Running Samples in DevCloud
If running a sample in the Intel DevCloud, remember that you must specify the compute node (cpu, gpu, fpga_compile, or fpga_runtime) and whether to run in batch or interactive mode.

For specific instructions, jump to [Run the sample in the DevCloud](#run-on-devcloud)

For more information, see the Intel® oneAPI Base Toolkit Get Started Guide ([https://devcloud.intel.com/oneapi/get-started/base-toolkit/](https://devcloud.intel.com/oneapi/get-started/base-toolkit/)).

## Build the `Simple Add` Program
> **Note**: If you have not already done so, set up your CLI
> environment by sourcing  the `setvars` script located in
> the root of your oneAPI installation.
>
> Linux*:
> - For system wide installations: `. /opt/intel/oneapi/setvars.sh`
> - For private installations: `. ~/intel/oneapi/setvars.sh`
>
> Windows*:
> - `C:\Program Files(x86)\Intel\oneAPI\setvars.bat`
> - For Windows PowerShell*, use the following command: `cmd.exe "/K" '"C:\Program Files (x86)\Intel\oneAPI\setvars.bat" && powershell'`

### Using Visual Studio Code*  (Optional)

You can use Visual Studio Code (VS Code) extensions to set your environment,
create launch configurations, and browse and download samples.

The basic steps to build and run a sample using VS Code include:
 - Download a sample using the extension **Code Sample Browser for Intel&reg; oneAPI Toolkits**.
 - Configure the oneAPI environment with the extension **Environment
   Configurator for Intel&reg; oneAPI Toolkits**.
 - Open a Terminal in VS Code (**Terminal>New Terminal**).
 - Run the sample in the VS Code terminal using the instructions below.

To learn more about the extensions and how to configure the oneAPI environment,
see the [Using Visual Studio Code with Intel&reg; oneAPI
Toolkits User Guide](https://www.intel.com/content/www/us/en/develop/documentation/using-vs-code-with-intel-oneapi/top.html).

#### Troubleshooting
If you receive an error message, troubleshoot the problem using the **Diagnostics Utility for Intel® oneAPI Toolkits**. The diagnostic utility provides configuration and system checks to help find missing dependencies, permissions errors and other issues. See the [Diagnostics Utility for Intel&reg; oneAPI Toolkits User Guide](https://www.intel.com/content/www/us/en/develop/documentation/diagnostic-utility-user-guide/top.html) for more information on using the utility.

### Build the `Simple Add` Sample
> **Note**: If you have not already done so, set up your CLI
> environment by sourcing  the `setvars` script in the root of your oneAPI installation.
>
> Linux*:
> - For system wide installations: `. /opt/intel/oneapi/setvars.sh`
> - For private installations: ` . ~/intel/oneapi/setvars.sh`
> - For non-POSIX shells, like csh, use the following command: `bash -c 'source <install-dir>/setvars.sh ; exec csh'`
>
> Windows*:
> - `C:\Program Files(x86)\Intel\oneAPI\setvars.bat`
> - Windows PowerShell*, use the following command: `cmd.exe "/K" '"C:\Program Files (x86)\Intel\oneAPI\setvars.bat" && powershell'`
>
> For more information on configuring environment variables, see [Use the setvars Script with Linux* or macOS*](https://www.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top/oneapi-development-environment-setup/use-the-setvars-script-with-linux-or-macos.html) or [Use the setvars Script with Windows*](https://www.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top/oneapi-development-environment-setup/use-the-setvars-script-with-windows.html).


### Using Visual Studio Code* (VS Code) (Optional)

You can use Visual Studio Code* (VS Code) extensions to set your environment,
create launch configurations, and browse and download samples.

The basic steps to build and run a sample using VS Code include:
1. Configure the oneAPI environment with the extension **Environment Configurator for Intel® oneAPI Toolkits**.
2. Download a sample using the extension **Code Sample Browser for Intel® oneAPI Toolkits**.
3. Open a terminal in VS Code (**Terminal > New Terminal**).
4. Run the sample in the VS Code terminal using the instructions below.

To learn more about the extensions and how to configure the oneAPI environment, see the 
[Using Visual Studio Code with Intel® oneAPI Toolkits User Guide](https://www.intel.com/content/www/us/en/develop/documentation/using-vs-code-with-intel-oneapi/top.html).

### On Linux*

#### Build for CPU and GPU

1. Change to the sample directory.

2. Build the program for Unified Shared Memory (USM).
    ```
    mkdir build
    cd build
    cmake .. -DUSM=1
    make cpu-gpu
    ```
3. Clean the program. (Optional)
   ```
   make clean
   ```

#### Build for FPGA

1.  Change to the sample directory.

2.  Build the program. (The provided targets match the recommended development flow.)
    ```
    mkdir build
    cd build
    cmake ..
    ```
    1. Compile for FPGA emulation.
       ```
       make fpga_emu
       ```
    2. Generate HTML performance reports.
       ```
       make report
       ```
       The reports reside at `simple-add_report.prj/reports/report.html`.

    3. Compile the program for FPGA hardware. (Compiling for hardware can take a long
   time.)
       ```
       make fpga
       ```

3. Clean the program. (Optional)
   ```
   make clean
   ```

### On Windows*

#### Build for CPU and GPU on the Command Line

1. Change to the sample directory.

2. Build the program for Unified Shared Memory (USM).
   ```
   mkdir build
   cd build
   cmake -G \"NMake Makefiles\" .. -DUSM=1
   nmake cpu-gpu
   ```
3. Clean the program. (Optional)
   ```
   nmake clean
   ```

#### Build for FPGA on the Command Line

>**Note**: Compiling to FPGA hardware on Windows* requires a third-party or custom Board Support Package (BSP) with Windows* support.

1. Change to the sample directory.

2. Build the program.

   ```
   mkdir build
   cd build
   cmake -G \"NMake Makefiles\" ..
   ```
    1. Compile for FPGA emulation.
       ```
       nmake fpga_emu
       ```
    2. Generate HTML performance reports.
       ```
       nmake report
       ```
       The reports reside at `simple-add_report.prj/reports/report.html`.

    3. Compile the program for FPGA hardware. (Compiling for hardware can take a long
   time.)
       ```
       nmake fpga
       ```

3. Clean the program. (Optional)
   ```
   nmake clean
   ```

## Run the `Simple Add` Program
### On Linux

#### Run for CPU and GPU

1. Change to the output directory.

2. Run the program for Unified Shared Memory (USM) and buffers.
    ```
    ./simple-add-buffers
    ./simple-add-usm
    ```
#### Run for FPGA

1.  Change to the output directory.

2.  Run for FPGA emulation.
    ```
    ./simple-add-buffers.fpga_emu
    ./simple-add-usm.fpga_emu
    ```
3. Run on FPGA hardware.
    ```
    ./simple-add-buffers.fpga
    ./simple-add-usm.fpga
    ```

### On Windows

#### Run for CPU and GPU

1. Change to the output directory.

2. Run the program for Unified Shared Memory (USM) and buffers.
    ```
    simple-add-usm.exe
    simple-add-buffers.exe
    ```

#### Run for FPGA

1.  Change to the output directory.

2.  Run for FPGA emulation.
    ```
    simple-add-buffers.fpga_emu.exe
    simple-add-usm.fpga_emu.exe
    ```
3. Run on FPGA hardware.
    ```
    simple-add-buffers.fpga.exe
    simple-add-usm.fpga.exe
    ```

### Application Parameters
The sample has no configurable parameters.

### Run the `Simple Add` Sample in Intel&reg; DevCloud
If running a sample in the Intel&reg; DevCloud, you must specify the compute node
(CPU, GPU, FPGA) and whether to run in batch or interactive mode. For more
information, see the Intel&reg; oneAPI Base Toolkit [Get Started
Guide](https://devcloud.intel.com/oneapi/get_started/).

### Example Output
```
simple-add output snippet changed to:
Running on device:        Intel(R) Gen9 HD Graphics NEO
Array size: 10000
[0]: 0 + 100000 = 100000
[1]: 1 + 100000 = 100001
[2]: 2 + 100000 = 100002
...
[9999]: 9999 + 100000 = 109999
Successfully completed on device.
```

### Build and Run the `Simple Add` Sample in Intel® DevCloud (Optional)

When running a sample in the Intel® DevCloud, you must specify the compute node (CPU, GPU, FPGA) and whether to run in batch or interactive mode.

>**Note**: Since Intel® DevCloud for oneAPI includes the appropriate development environment already configured, you do not need to set environment variables.

Use the Linux instructions to build and run the program.

You can specify a GPU node using a single line script.

```
qsub  -I  -l nodes=1:gpu:ppn=2 -d .
```

- `-I` (upper case I) requests an interactive session.
- `-l nodes=1:gpu:ppn=2` (lower case L) assigns one full GPU node. 
- `-d .` makes the current folder as the working directory for the task.

  |Available Nodes           |Command Options
  |:---                      |:---
  |GPU	                     |`qsub -l nodes=1:gpu:ppn=2 -d .`
  |CPU	                     |`qsub -l nodes=1:xeon:ppn=2 -d .`
  |FPGA Compile Time         |`qsub -l nodes=1:fpga_compile:ppn=2 -d .`
  |FPGA Runtime (Arria 10)   |`qsub -l nodes=1:fpga_runtime:arria10:ppn=2 -d .`


>**Note**: For more information on how to specify compute nodes, read [Launch and manage jobs](https://devcloud.intel.com/oneapi/documentation/job-submission/) in the Intel® DevCloud for oneAPI Documentation.

Only `fpga_compile` nodes support compiling to FPGA. When compiling for FPGA hardware, increase the job timeout to **24 hours**.

Executing programs on FPGA hardware is only supported on `fpga_runtime` nodes of the appropriate type, such as `fpga_runtime:arria10`.

Neither compiling nor executing programs on FPGA hardware are supported on the login nodes. For more information, see the Intel® DevCloud for oneAPI [*Intel® oneAPI Base Toolkit Get Started*](https://devcloud.intel.com/oneapi/get_started/) page.


## License
Code samples are licensed under the MIT license. See
[License.txt](https://github.com/oneapi-src/oneAPI-samples/blob/master/License.txt)
for details.

Third party program Licenses can be found here:
[third-party-programs.txt](https://github.com/oneapi-src/oneAPI-samples/blob/master/third-party-programs.txt).
