# Building Modeling
## Introduction
Accurate modeling of building energy systems is a cornerstone of modern smart building research, enabling applications ranging from energy consumption prediction, demand response, and optimal control. As buildings account for more than 30% of global energy consumption, developing reliable and efficient building models has become increasingly critical for achieving sustainability goals.

Building modeling approaches are broadly categorized into three paradigms, each reflecting a different balance between physical interpretability and data-driven flexibility:
| Model Paradigm | Description |
|---|---|
| **[⚪ White-box Models](#white-box-models)** | Physics-based models that explicitly encode building dynamics using first-principles equations, such as thermodynamics, heat transfer, and HVAC system behavior. They are highly interpretable and generalizable, but usually require detailed building specifications and careful calibration. |
| **[⚫ Black-box Models](#black-box-models)** | Data-driven models that learn building behavior directly from operational data without explicitly modeling the underlying physical processes. They can achieve strong predictive performance, especially with large-scale sensor data, but often lack interpretability and physical consistency. |
| **[🔘 Gray-box Models](#gray-box-models)** | Hybrid models that combine simplified physical structures with data-driven parameter estimation. They aim to balance interpretability and flexibility, making them useful when partial physical knowledge and measured data are both available. |

## White-box Models
White-box models describe building behavior based on known physical principles. They explicitly model how heat, air, energy, and control signals flow through a building and its HVAC systems, using equations derived from thermodynamics, heat transfer, fluid dynamics, and system control. Unlike purely data-driven models, white-box models do not treat the building as an unknown mapping from inputs to outputs; instead, they attempt to reproduce the actual physical processes that determine indoor conditions and energy consumption.

A key advantage of white-box models is interpretability. Most parameters correspond to real physical quantities, such as wall insulation, thermal mass, ventilation rate, or equipment efficiency. This makes them valuable for simulation, diagnosis, control design, and what-if analysis. At the same time, they often require substantial domain knowledge, detailed building specifications, and calibration effort, which can make them costly to develop and maintain.

Several mature simulation platforms have been widely used to develop and analyze white-box building models. Among them, EnergyPlus, TRNSYS, and OpenStudio are representative tools that support physics-based modeling of building thermal behavior, HVAC systems, and energy performance.

**[EnergyPlus](https://energyplus.net/)** is one of the most widely used whole-building energy simulation engines. It models building heat transfer, thermal zones, internal loads, weather interactions, ventilation, and HVAC system dynamics based on detailed physical equations. EnergyPlus is particularly useful for evaluating building energy consumption, indoor thermal conditions, and the impact of design or control strategies. However, creating accurate EnergyPlus models often requires detailed information about building geometry, envelope materials, schedules, HVAC configurations, and system parameters.

**[TRNSYS](https://www.trnsys.com/)** is a flexible transient system simulation environment designed for modeling dynamic energy systems. Unlike tools focused only on whole-building simulation, TRNSYS uses a modular structure in which different physical components, such as buildings, solar collectors, heat pumps, thermal storage, and control units, can be connected together. This makes it especially suitable for studying complex energy systems, renewable energy integration, and customized HVAC configurations. Its flexibility is a major advantage, but it also requires users to carefully define system components, connections, and parameters.

**[OpenStudio](https://openstudio.net/)** provides a software development kit and workflow environment built around EnergyPlus. It simplifies the process of creating, editing, and managing building energy models by offering higher-level interfaces, model transformation tools, and integration with parametric analysis workflows. OpenStudio is often used to reduce the modeling burden of EnergyPlus and to support large-scale simulation studies, automated model generation, and building energy analysis. While it improves usability and workflow management, the underlying model still relies on the physical assumptions and input requirements of EnergyPlus.

**[Modelica Buildings Library](https://simulationresearch.lbl.gov/modelica/)** is an equation-based modeling framework widely used for simulating complex physical systems, including building thermal dynamics, HVAC systems, and energy control applications. The Buildings Library, developed by Lawrence Berkeley National Laboratory, provides reusable component models for building envelopes, HVAC equipment, control systems, and district energy systems. Compared with conventional whole-building simulation tools, Modelica-based modeling offers greater flexibility for representing dynamic interactions between subsystems and is particularly useful for control-oriented simulation, co-simulation, and model predictive control studies. However, it usually requires stronger modeling expertise and careful configuration of system components and equations.

**[IDA ICE](https://www.equa.se/en/ida-ice)** is a commercial building performance simulation tool for modeling indoor climate, energy use, ventilation, daylight, and HVAC system operation. It is commonly used for evaluating thermal comfort, building energy performance, and system-level design alternatives under realistic operating conditions. IDA ICE provides a more integrated graphical modeling environment, which can make model setup and analysis more accessible compared with purely equation-based workflows. At the same time, accurate simulation still depends on detailed building inputs, proper system representation, and calibration against measured or expected performance.

In summary, white-box models are powerful tools for physically interpretable building simulation and performance analysis. However, they often require detailed input data, domain expertise, and calibration effort, which can make them difficult to scale. This trade-off provides the motivation for black-box and gray-box modeling approaches that offer greater flexibility in data-rich building applications.

## Black-box Models
Black-box models learn building thermal dynamics directly from operational data without explicitly modeling the underlying physical processes such as heat conduction, convection, or thermal storage. Instead of constructing equations based on building geometry, material properties, or HVAC system configurations, these models rely on historical input-output data to approximate the mapping between driving variables (e.g., outdoor temperature, solar radiation, HVAC control signals, internal heat gains) and thermal responses (e.g., indoor temperature, zone thermal load).

The general form of a black-box thermal dynamic model can be expressed as:
$$T_{k+1} = f(T_k, T_{k-1}, \dots, u_k, u_{k-1}, \dots, \omega_k, \omega_{k-1}, \dots)$$
where $T_k$ denotes the indoor temperature at time step $k$, $u_k$ represents the control inputs (e.g., HVAC, heating/cooling power, supply air temperature, fan speed), $\omega_k$ denotes the external disturbances (e.g., outdoor temperature, solar irradiance, occupancy), and $f(\cdot)$ is a nonlinear function learned entirely from data.

Black-box models can achieve strong predictive performance, particularly when large-scale sensor data is available. However, they typically 
### Model Structures

### Model Training

## Gray-box Models
Representative approaches include Physics-Informed Neural Networks (PINNs), which embed governing differential equations as soft constraints during neural network training, and Resistance-Capacitance (RC) thermal network models, which abstract building thermal dynamics into compact equivalent circuits whose parameters are identified from data. These hybrid methods offer improved sample efficiency and interpretability compared to pure black-box approaches, while relaxing the detailed input requirements of white-box simulators.
