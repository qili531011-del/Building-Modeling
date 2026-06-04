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

Black-box models can achieve strong predictive performance, particularly when large-scale sensor data is available. However, they typically  lack interpretability and do not guarantee physical consistency (e.g., energy conservation, monotonic temperature response to heating input). Their generalization to unseen operating conditions or building configurations is also limited compared to physics-based approaches.

This section covers two aspects of black-box modeling for building thermal dynamics. The first part introduces representative model structures commonly used to approximate the thermal dynamics function 
$f(\cdot)$, ranging from traditional machine learning methods such as support vector regression and gradient boosting trees to deep learning architectures such as MLP, LSTM, GRU, and TCN. The second part discusses model training strategies, including federated learning, end-to-end learning, and explainable AI.

### Model Structures

Black-box thermal dynamic models differ mainly in the choice of function approximator used to learn the nonlinear mapping $f(\cdot)$. In building thermal modeling, the input to the model is usually constructed from historical indoor temperatures, control inputs, weather variables, occupancy-related variables, and other measurable disturbances. Depending on how temporal dependencies are represented, black-box model structures can be broadly divided into traditional machine learning models and deep learning models.

#### Support Vector Regression

Support Vector Regression (SVR) is one of the commonly used traditional machine learning methods for building thermal dynamics prediction. SVR approximates the nonlinear relationship between input features and thermal responses by mapping the original input space into a high-dimensional feature space through kernel functions. A typical SVR-based thermal model can be written as:

$$
T_{k+1} = f_{\text{SVR}}(x_k)
$$

where $x_k$ is the feature vector constructed from historical temperatures, HVAC control signals, weather disturbances, and occupancy-related variables, for example:

$$
x_k = [T_k, T_{k-1}, u_k, u_{k-1}, \omega_k, \omega_{k-1}]
$$

The kernel function allows SVR to capture nonlinear thermal behavior without explicitly defining the physical heat transfer process. Common kernel choices include radial basis function, RBF, kernels and polynomial kernels.

SVR is suitable for small- to medium-scale datasets and often provides robust prediction performance when the amount of training data is limited. However, its performance depends strongly on feature engineering, kernel selection, and hyperparameter tuning. In addition, SVR does not naturally model long-term temporal dependencies unless lagged variables are manually included in the input feature vector.

#### Tree-based Ensemble Models

Tree-based ensemble models, such as Random Forest, Gradient Boosting Decision Trees, XGBoost, LightGBM, and CatBoost, are widely used for black-box building thermal modeling due to their strong nonlinear fitting ability and relatively low requirement for data preprocessing. These models approximate the thermal dynamics function as an ensemble of decision trees:

$$
T_{k+1} = f_{\text{tree}}(x_k)
$$

where $x_k$ contains current and historical measurements of temperature, control inputs, weather conditions, internal gains, and calendar variables.

Gradient boosting models are particularly effective because they sequentially train multiple weak learners to reduce prediction errors. In thermal dynamic modeling, they can capture nonlinear effects such as the influence of outdoor temperature, solar radiation, HVAC operation, and occupancy on indoor temperature evolution.

Compared with neural networks, tree-based models are easier to train and can provide feature importance indicators, which offer a limited degree of interpretability. They are also effective for tabular building operation datasets. However, similar to SVR, temporal dynamics must usually be introduced manually through lagged features, rolling statistics, or window-based input construction. Their extrapolation capability is also limited when the operating condition differs significantly from the training data distribution.

#### Multi-layer Perceptron

Multi-layer Perceptron (MLP) is a basic feedforward neural network structure that can approximate complex nonlinear mappings between input features and thermal responses. An MLP-based thermal dynamic model can be expressed as:

$$
T_{k+1} = f_{\text{MLP}}(x_k; \theta)
$$

where $\theta$ denotes the trainable parameters of the neural network; the feature vector $x_k$ usually includes lagged indoor temperatures, HVAC control variables, weather disturbances, occupancy indicators, and time-related features.

A typical MLP consists of an input layer, several hidden layers with nonlinear activation functions, and an output layer. The hidden layers learn nonlinear combinations of input variables, allowing the model to represent complex thermal response patterns.

MLP models are simple, flexible, and easy to implement. They are suitable when the thermal dynamics can be approximated from a fixed-length input window. However, because MLPs do not contain an explicit recurrent or temporal mechanism, the temporal dependency of building thermal dynamics must be encoded through handcrafted lagged features. Therefore, the prediction performance of MLP models is highly affected by the choice of input window length and feature construction strategy.

#### Recurrent Neural Networks

Building thermal dynamics are inherently time-dependent because indoor temperature is affected not only by current inputs but also by previous thermal states and accumulated heat storage effects. Recurrent Neural Networks (RNNs) are designed to model sequential data by maintaining hidden states that carry information from previous time steps.

A general recurrent thermal model can be written as:

$$
h_k = \phi(h_{k-1}, x_k)
$$

$$
T_{k+1} = g(h_k)
$$

where $h_k$ is the hidden state, $x_k$ is the input vector at time step $k$, and $\phi(\cdot)$ and $g(\cdot)$ are nonlinear functions learned from data.

Basic RNNs can model temporal dependencies, but they often suffer from vanishing or exploding gradient problems when learning long-term dependencies. Since building thermal dynamics may involve delayed responses over several hours, more advanced recurrent architectures such as LSTM and GRU are commonly preferred.

#### Long Short-Term Memory Networks

Long Short-Term Memory (LSTM) is a recurrent neural network architecture designed to capture long-term temporal dependencies. LSTM introduces memory cells and gating mechanisms to control the flow of information over time. This makes it suitable for modeling building thermal inertia, delayed HVAC effects, and the accumulated influence of disturbances.

For building thermal dynamics, an LSTM model takes a sequence of historical input features as input:

$$
X_k = [x_{k-n+1}, x_{k-n+2}, \dots, x_k]
$$

and predicts the future indoor temperature:

$$
T_{k+1} = f_{\text{LSTM}}(X_k; \theta)
$$

where $n$ is the sequence length.

LSTM models can automatically learn temporal dependencies from historical data without requiring extensive manual construction of lagged features. They are especially useful for multi-step temperature prediction and predictive control applications. However, LSTM models usually require larger datasets and more careful training than traditional machine learning models. They may also be computationally more expensive and less interpretable.

#### Gated Recurrent Unit Networks

Gated Recurrent Unit (GRU) is another recurrent neural network architecture that uses gating mechanisms to capture temporal dependencies. Compared with LSTM, GRU has a simpler structure with fewer parameters, typically using update and reset gates instead of separate input, forget, and output gates.

A GRU-based thermal model can be represented as:

$$
T_{k+1} = f_{\text{GRU}}(X_k; \theta)
$$

where $X_k$ is the sequence of historical temperature, control, and disturbance variables.

GRU models often achieve similar predictive performance to LSTM models while requiring lower computational cost and less training time. This makes GRU attractive for building thermal modeling tasks where the dataset size is moderate or where real-time implementation is needed. Similar to LSTM, GRU can represent delayed thermal responses and temporal correlations, but its learned representation remains difficult to physically interpret.

#### Temporal Convolutional Networks

Temporal Convolutional Networks (TCNs) provide an alternative to recurrent architectures for sequence modeling. Instead of processing time-series data recurrently, TCNs use one-dimensional causal convolutions to capture temporal dependencies. A causal convolution ensures that the prediction at time step $k+1$ only depends on current and past information, avoiding information leakage from future data.

A TCN-based thermal dynamic model can be written as:

$$
T_{k+1} = f_{\text{TCN}}(X_k; \theta)
$$

where $X_k$ denotes a historical input sequence.

TCNs often use dilated convolutions to enlarge the receptive field, allowing the model to capture long-term dependencies without very deep networks. This is useful for building thermal dynamics because the indoor temperature response may depend on HVAC operation and weather conditions over a relatively long historical period.

Compared with LSTM and GRU, TCNs are easier to parallelize during training and can be more stable for long sequences. They are suitable for both one-step-ahead and multi-step-ahead temperature prediction. However, the choice of receptive field, dilation factor, kernel size, and sequence length can significantly influence model performance.

#### Comparison of Representative Structures

Different black-box model structures have different advantages and limitations for building thermal dynamic modeling. Traditional machine learning models, such as SVR and tree-based ensemble models, are generally easier to train and perform well on tabular datasets with carefully designed features. However, they usually require manual construction of lagged variables to represent temporal dependencies.

Deep learning models, such as MLP, LSTM, GRU, and TCN, provide stronger capability for learning nonlinear and temporal patterns from large-scale operational data. MLPs are simple and flexible but rely on fixed-window feature construction. LSTM and GRU can naturally model sequential dependencies and thermal inertia. TCNs can efficiently capture long-range temporal relationships through causal and dilated convolutions.

A summary of representative black-box model structures is shown below.

| Model Structure | Main Characteristics | Advantages | Limitations |
|---|---|---|---|
| SVR | Kernel-based nonlinear regression | Effective for small datasets; robust prediction | Sensitive to kernel and hyperparameters; temporal features need manual design |
| Random Forest / Gradient Boosting Trees | Ensemble of decision trees | Strong performance on tabular data; limited feature interpretability | Weak extrapolation; temporal dependency requires lagged features |
| MLP | Feedforward neural network | Simple structure; flexible nonlinear approximation | No explicit temporal memory; depends on input window design |
| LSTM | Recurrent neural network with memory cells | Captures long-term dependencies and thermal inertia | Requires more data; higher computational cost; low interpretability |
| GRU | Simplified recurrent neural network | Similar to LSTM with fewer parameters; efficient training | Still difficult to interpret physically |
| TCN | Causal and dilated temporal convolution | Parallel training; effective for long sequences | Requires careful design of receptive field and convolution settings |

Overall, the selection of a black-box model structure depends on the available data volume, sampling resolution, prediction horizon, computational resources, and application scenario. For short-term temperature prediction with limited data, SVR or gradient boosting models may be sufficient. For large-scale time-series datasets and applications requiring multi-step prediction, LSTM, GRU, and TCN models are often more suitable.
### Model Training

## Gray-box Models
Representative approaches include Physics-Informed Neural Networks (PINNs), which embed governing differential equations as soft constraints during neural network training, and Resistance-Capacitance (RC) thermal network models, which abstract building thermal dynamics into compact equivalent circuits whose parameters are identified from data. These hybrid methods offer improved sample efficiency and interpretability compared to pure black-box approaches, while relaxing the detailed input requirements of white-box simulators.
