# openvre-tool-api

## Introduction
This library implements the specifications of a new tool in the [openVRE](https://github.com/inab/openVRE.git) enviroment.
It provides a simple programming paradigm to develop tools for the VRE.

The main goals that this proposal aims to achieve are:

1. Achieve horizontal interoperability by defining a "Tool" as a specific
functionality with precise inputs and outputs, which must both comply with the
Data Management Plan (DMP). Tools are implemented as a thin wrappers over
existing software, in order to facilitate integrating existing tools in the
VRE.

2. Achieve vertical interoperability by using PyCOMPSs, and allowing
developers to specify the execution enviroment requirements for each tool by
using PyCOMPSs "constraints" decorator. Although written with task-based
programming in mind, this library allows execution of Tools outside of the
COMPSs runtime.

3. Simplify the construction of workflows, by conceiving tools such that it is
straightforward to combine them in Workflows; in particular by using PyCOMPSs
"task" decorator and the PyCOMPSs runtime as the workflow scheduler.

## Implementation overview
The 'basic_modules' contains the basic entities of openvre-tool-api:
1. Tool:
	Is the top-level wrapper for tools within the VRE; each tool is defined
	by its input and output formats, and by its specific requirements on the
	execution environment. Tools should implement a "run" method, that defines
	operations needed to get from input to output. The "run" method calls other
	methods which are decorated using PyCOMPSs "@task" and "@constraints",
	allowing tool developers to define the workflow and execution environment.
    See also Workflow.
2. App:
	Is the main entry point to the tools layer of the VRE; it deals with heterogeneity
	in the way Tools are run, in terms of filesystem access, runtime environment,
	error reporting, and more. Therefore, Apps are compatible with all Tools.
	Apps implement a "launch" method, which prepares and runs a single instance of Tool.
	The "apps" module provides some example Apps for straightforward cases:

	- *LocalApp*: assumes files to be locally accessible;

	- *PyCOMPSsApp*: specific for Tools using PyCOMPSs;

	- *WorkflowApp*: inherits from both of the above, and implements the ability
	  to run Workflows.

	- *JSONApp*: inherits from WorkflowApp, and implements the ability to read
	  run configuration from JSON files, and write results in a JSON file; JSON
      formats used are those provided, and accepted, by the VRE.

3. Metadata:
   Class that contains extra information about files.

The 'utils' module contains useful functions for performing common tasks in Tool
execution. In particular it contains 'logger', the logging facility of openvre-tool-api;
it provides a unified way of sending messages to the VRE.

See the documentation for the classes for more information.

## Installation

Directly from GitHub:

```bash
pip install git+https://github.com/inab/openvre-tool-api.git
```

## Examples

The repository [vre_sample_tool](https://github.com/inab/vre_sample_tool.git) showcase the functionalities of this Wrapper by implementing
a simple application. It consists on a CWL Tool execution.

