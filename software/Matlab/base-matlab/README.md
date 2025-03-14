<!--
   Copyright 2024, Center for High Throughput Computing, University of Wisconsin - Madison

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
-->

# [base-matlab](/software/Matlab/base-matlab)

Container definition files for building Matlab.

For instructions on selecting the version of Matlab to use, see the instructions in the [parent README](../README.md#choosing-the-version-of-matlab).

If you want to install additional toolboxes, see the example that installs the Symbolic Math toolbox: [symbolic-math](../symbolic-math/README.md).

**By using these definition files, you are agreeing to the terms and conditions for the use of Matlab as licensed by CHTC, for use at CHTC.**

## [base-matlab.def](base-matlab.def)

| | | |
| ---: | :--- | :--- |
| *Type* | **Apptainer** | |
| *OS* | Ubuntu 22.04 | |
| *Base image* | **mathworks/matlab-deps:r2022b** | *DockerHub* |
| *Updated* | 2025-01-16 | *Amber Lim* |
| *Last tested on HTC* | 2025-01-16 | *Amber Lim* |
| *Last tested on HPC* | - | - |

## [Dockerfile](Dockerfile)

| | | |
| ---: | :--- | :--- |
| *Type* | **Docker** | |
| *OS* | Ubuntu 22.04 | |
| *Base image* | **mathworks/matlab-deps:r2022b** | *DockerHub* |
| *Updated* | 2024-05-07 | *Andrew Owen* |
| *Last tested on HTC* | 2024-05-06 | *Andrew Owen* |
| *Last tested on HPC* | - | - |
