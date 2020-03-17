# BenchBot Software Stack

![benchbot_web](./docs/benchbot_web.gif)

The BenchBot Software Stack is a collection of software packages that allow end users to control robots in real or simulated environments with a simple python API. It leverages the simple "observe, act, repeat" approach to robot problems prevalent in reinforcement learning communities ([OpenAI Gym](https://gym.openai.com/) will find the BenchBot interface extremely familiar).

BenchBot has been created primarily as a tool to broach the research challenges faced by the Semantic Scene Understanding community (both in understanding a scene in simulation, & transferring algorithms to real world systems). The "bench" in "BenchBot" refers to benchmarking, with our goal to provide a system that makes it easy to benchmark the performance of novel algorithms in realistic 3D simulation & on real robot platforms. Users performing tasks other than Semantic Scene Understanding (like object detection, 3D mapping, RGB to depth reconstruction, active vision, etc.) will also find elements of the BenchBot Software Stack useful. 

This repository contains the software stack needed to develop solutions for BenchBot challenges on your local machine. It installs & configures a significant amount of software for you, wraps software in stable Docker images (~120GB), and provides simple interaction with the stack through 4 basic scripts: `benchbot_install`, `benchbot_run`, `benchbot_submit`, & `benchbot_eval`.

## System recommendations & requirements

The BenchBot Software Stack is designed to run seamlessly on a wide number of system configurations (currently limited to Ubuntu 18.04+). System hardware requirements are relatively high due to the nature of software being run for simulation (Unreal Engine, Nvidia Isaac, Vulkan, etc.):

- Nvidia Graphics card (GeForce GTX 1080 minimum, Titan XP+ / GeForce RTX 2070+ recommended)
- CPU with multiple cores (Intel i7-6800K minimum)
- 32GB+ RAM
- 128GB+ spare storage (an SSD storage device **strongly** recommended)

Once your system has the above requirements it should be ready to install. The install script analyses your system configuration & offers to install any missing software components interactively. The list of 3rd party software components involved includes:

- Nvidia Driver (4.18+ required, 4.30+ recommended)
- CUDA with GPU support (10.0+ required, 10.1+ recommended)
- Docker Engine - Community Edition (19.03+ required, 19.03.2+ recommended)
- Nvidia Container Toolkit (1.0+ required, 1.0.5 recommended)
- ISAAC 2019.2 SDK (requires an Nvidia developer login)

## Managing your installation

Installation is simple:

```
u@pc:~$ git clone https://github.com/RoboticVisionOrg/benchbot
u@pc:~$ benchbot/install
```

Any missing software components, or configuration issues with your system, should be detected by the install script & resolved interactively. 

The BenchBot Software Stack will frequently check for updates & can update itself automatically. To update simply run the install script again (add the `--force-clean` flag if you would like to install from scratch):

```
u@pc:~$ benchbot_install
```

If you decide to uninstall the BenchBot Software Stack, run:

```
u@pc:~$ benchbot_install --uninstall
```

## Getting started

Getting a solution up & running with BenchBot is as simple as 1,2,3:

1. Run a simulator with the BenchBot Software Stack by selecting a valid environment & task definition. For example (also see `--help`, `--list-tasks`, & `--list-envs` for more details of options):

    ```
    u@pc:~$ benchbot_run --env miniroom:1 --task semantic_slam:active:ground_truth
    ```

2. Create a solution to a BenchBot task, & run it against the software stack. The `<BENCHBOT_ROOT>/examples` directory contains some basic "hello_world" style solutions. For example, the following commands run the `hello_active` example in either a container or natively respectively (see `--help` for more details of options):

    ```
    u@pc:~$ benchbot_submit --containerised <BENCHBOT_ROOT>/examples/hello_active/ 
    ```
    ```
    u@pc:~$ benchbot_submit --native python <BENCHBOT_ROOT>/examples/hello_active/hello_active
    ```

3. Evaluate the performance of your system either directly, or automatically after your submission completes respectively:

    ```
    u@pc:~$ benchbot_eval <RESULTS_FILENAME>
    ```
    ```
    u@pc:~$ benchbot_submit --evaluate-results --native python <MY_SOLUTION>
    ```

See [benchbot_examples](https://github.com/RoboticVisionOrg/benchbot_examples) for more examples & further details of how to get up & running with the BenchBot Software Stack.

## Components of the BenchBot Software Stack

The BenchBot Software Stack is split into a number of standalone components, each with their own Github repository & documentation. This repository glues them all together for you into a working system. The components of the stack are:

- **[benchbot_simulator](https://github.com/RoboticVisionOrg/benchbot_simulator):** realistic 3D simulator employing Nvidia's Isaac framework, in combination with Unreal Engine environments
- **[benchbot_supervisor](https://github.com/RoboticVisionOrg/benchbot_supervisor):** a HTTP server facilitating communication between user-facing interfaces & the low-level ROS components of a simulator or real robot
- **[benchbot_api](https://github.com/RoboticVisionOrg/benchbot_api):** user-facing Python interface to the BenchBot system, allowing the user to control simulated or real robots in simulated or real world environments through simple commands
- **[benchbot_examples](https://github.com/RoboticVisionOrg/benchbot_examples):** a series of example submissions that use the API to drive a robot interactively, autonomously step through environments, evaluate dummy results, attempt semantic slam, & more
- **[benchbot_eval](https://github.com/RoboticVisionOrg/benchbot_eval):** Python library for evaluating the performance in a task based on the results produced by a submission
