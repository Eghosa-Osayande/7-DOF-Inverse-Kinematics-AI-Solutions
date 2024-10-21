# A Comparative Analysis of AI and Optimization Approaches for Inverse Kinematics in a 7-DOF Robotic Arm

| *Figure 1: Robot schematics for joint angles (0°,0°,0°,-60°,0°,0°,0°)* | 
| ------- |
|![image](https://github.com/user-attachments/assets/66defe73-baf4-4a0f-9a03-f82e63e3cd21)|

## Abstract

Inverse Kinematics (IK) is a critical challenge in robotics, essential for determining the joint configurations necessary for a robotic arm to achieve specific positions and orientations of its end effector. This study presents a comparative analysis of various artificial intelligence (AI) models and optimization techniques applied to the inverse kinematics problem for a 7-degree-of-freedom (DOF) robotic arm. We explore two AI models—Decision Tree Regressor (DTR) and Multi-Layer Perceptron (MLP) Regressor—alongside four optimization strategies: Genetic Algorithms (GA), Differential Evolution (DE), Particle Swarm Optimization (PSO), and Simulated Annealing (SA). Our methodology involves modelling the forward kinematics of the robotic arm and generating a comprehensive dataset, which allows us to train and validate the models. By systematically tuning hyperparameters for each method and conducting cross-comparative analyses based on the Mean Absolute Error (MAE) between actual and predicted positions, we assess the effectiveness of each approach. The findings indicate that optimization techniques, particularly DE, outperform traditional AI models in terms of accuracy and efficiency in solving the inverse kinematics problem for 7-DOF robotic arms, highlighting the advantages of integrating AI with heuristic optimization strategies in robotics applications.

## METHODOLOGY

The methodology outlined in this paper is divided into the following sections:
- Forward Kinematics Modeling of a 7-DOF Robotic Manipulator
- Model Development and Hyperparameter Optimization

### Forward Kinematics Modeling of a 7-DOF Robotic Manipulator

The robotic manipulator examined in this study consists of seven revolute joints and six links, each measuring 1 meter, as depicted in Figure 1. Joint 1 is anchored to the base of the manipulator, allowing for rotation between 0 degrees and 360 degrees around the z-axis. The other six joints have rotational ranges extending from -90 to 0 degrees. Table 1 details each joint's rotational limits and their corresponding Denavit-Hartenberg (DH) parameters.

The DH parameters define the relationship between each joint and its predecessor, establishing the linkage between the world frame and the end effector frame. This relationship is formulated by multiplying the 4x4 transformation matrices of the seven joints, as illustrated in the equation below.

A representative transformation matrix is displayed below.

### Table 1: DH Parameters

| Joint   | alpha (degrees) | a (meters) | d (meters) | theta (degrees) | theta max (degrees) | theta min (degrees) |
|---------|------------------|------------|------------|------------------|---------------------|---------------------|
| Joint 1 | 90               | 0          | 1          | th1              | 360                 | 0                   |
| Joint 2 | 0                | 1          | 0          | th2              | 0                   | -90                 |
| Joint 3 | 0                | 1          | 0          | th3              | 0                   | -90                 |
| Joint 4 | 0                | 1          | 0          | th4              | 0                   | -90                 |
| Joint 5 | 0                | 1          | 0          | th5              | 0                   | -90                 |
| Joint 6 | 0                | 1          | 0          | th6              | 0                   | -90                 |
| Joint 7 | 0                | 1          | 0          | th7              | 0                   | -90                 |


### Model Development and Hyperparameters Optimization

The optimization algorithms evaluated in this research include Genetic Algorithm (GA), Differential Evolution (DE), Particle Swarm Optimization (PSO), and Simulated Annealing (SA). All these algorithms fall under the meta-heuristic optimization category. While GA, DE, and PSO are population-based methods, SA operates on a probabilistic basis. The implementations of these algorithms utilized the “scipy-opt” Python library.

For machine learning, the study employed Multi-Layer Perceptron (MLP) Regressor and Decision Tree Regressor (DTR). MLP is a type of feedforward neural network characterized by one or more hidden layers and is typically used for regression tasks. Decision trees, on the other hand, are supervised machine learning algorithms suitable for both classification and regression tasks. In classification, a decision tree predicts the class label based on input features, while in regression, it estimates a continuous target variable. Given that the Inverse Kinematics problem is a regression task, the Decision Tree Regressor was utilized. The "sklearn" library was used for implementing both MLP and DTR models.

Table 2 outlines the hyperparameters adjusted to achieve optimal solutions for the inverse kinematics of the manipulator using the various optimization methods.

#### Table 2: Optimization Model Hyperparameters

| GA | DE | PSO | SA |
| :---- | :---- | :---- | :---- |
| Objective Function | Objective Function | Objective Function | Objective Function |
| Population size | Population size | Population size | Maximum and Minimum Temperatures |
| Maximum number of iterations | Maximum number of iterations | Maximum number of iterations | Number of iterations |
| Mutation probability | Mutation probability | Cognitive Parameter  |  |
|  |  | Social Parameter |  |
|  |  | Inertia Weight |  |

Table 3 details the hyperparameters varied during the training of AI models to minimize errors in the position and posture predictions.

#### Table 3: AI Model Hyperparameters

| MLP | DTR |
| :---- | :---- |
| Number of hidden layers | Maximum depth |
| Size of hidden layers | Maximum Features |
| Activation function | Minimum samples split |
| Solver for weight optimisation | Minimum samples leaf |
| Batch size |  |
| Learning rate |  |


## Experiment And Results

#### Experiment Setup
This project made use of jupyter note books running in python 3.9.6 environment. The following dependencies were used through out the experiment
`
jupyter==1.0.0 
numpy==1.26.4 
pandas==2.2.1 
scikit-learn==1.4.1.post1 s
cikit-opt==0.6.6 
matplotlib==3.8.3
`

### Dataset Generation and Preprocessing

The training dataset was created by randomly generating angles for each joint within their defined constraints. For each joint configuration, the corresponding end effector position was computed using forward kinematics, resulting in a dataset containing 1,000 entries. This dataset was then divided into two subsets: 800 samples for training and 200 samples for validation. Figure 2 illustrates the spatial distribution of the training data in 3D space. Each position entry includes the following features:
- End effector position in Cartesian coordinates (x, y, z)
- End effector position in polar coordinates (rho, theta, phi)
- End effector position in spherical coordinates (rho, theta, phi)

| *Figure 2: Distribution of points in the generated dataset* | 
| ------- |
| ![image](https://github.com/user-attachments/assets/3a68fed7-a9d8-4025-9e86-3d66445d59b1)|

### Hyper-parameter iteration and selection
This subsection explores the variation of multiple hyperparameters for each optimization and machine learning algorithm while maintaining the other parameters constant. The impact of these changes on the MAE value was monitored. Subsequently, the most effective estimator for each category was identified based on the hyperparameter configurations that resulted in the lowest MAE values.

**Genetic Algorithm**
| Comment | Illustration |
| ------- | ------------ |
| Varying the population size showed minimal MAE variation within population sizes of 50 to 200, but a significant increase was observed within the range of 10 to 50. **A population of 200 was selected.** | ![image](https://github.com/user-attachments/assets/8f47171f-c84e-493d-b210-2a629b644fa7)|
| When varying the maximum iteration count, **a value of 50 was chosen.** Beyond 30, increasing the maximum iteration did not significantly affect the MAE. | ![image](https://github.com/user-attachments/assets/7e1abc46-68a3-4890-adab-b250c755f5b0)|
| For the mutation probability, **a value of 0.1 was selected.** The trend observed indicated that the MAE value increased as the mutation probability increased. | ![image](https://github.com/user-attachments/assets/2ae14a10-f714-4a5d-a6cd-4a83e909c3ba)|

**Differential Evolution**
| Comment | Illustration |
| ------- | ------------ |
| Keeping other parameters constant while varying the population size led to **a value of 200 being selected.** Lower MAE values were obtained for higher population sizes. |![image](https://github.com/user-attachments/assets/d0d35cd9-3018-451d-a911-12f3e92a92fb)|
| Variations in the maximum iteration count showed no effect on the MAE value. **A value of 50 was selected.** | ![image](https://github.com/user-attachments/assets/85ba4e0b-e3b6-40be-b8d6-18cdc8ceda2f)|
| A mutation probability closer to 1 resulted in lower MAE values. **A value of 0.9 was selected.** | ![image](https://github.com/user-attachments/assets/e1bab322-41f7-45a5-9a60-23cfc2fac57b)|

**Particle Swarm Optimization**
| Comment | Illustration |
| ------- | ------------ |
| Increasing the population size resulted in lower MAE values. **A size of 200 was selected.** | ![image](https://github.com/user-attachments/assets/8d0bbeda-c5c5-417f-9df6-8285606ff8f6)|
| Higher maximum iteration values correlated with lower MAE values, with minimal improvement noted beyond a **maximum iteration count of 100.** | ![image](https://github.com/user-attachments/assets/0f73d0c1-ffa4-4cee-b1f8-5285a9e3fd3f)|
| Variations in the inertia parameter did not affect the MAE value. **An inertia of 0.1 was selected.** | ![image](https://github.com/user-attachments/assets/b7f47d98-c9dd-46ee-b1f5-b3e161be4e91)|
| The social parameter exhibited slight variations with the cognitive parameter. Optimal social parameter values with lower MAE were found between 0.5 and 0.6. **A social parameter of 0.5 and a cognitive parameter of 0.25 were selected.** | ![image](https://github.com/user-attachments/assets/fdf9589d-fe02-4be2-b42d-6454739ac643)|

**Simulated Annealing**
| Comment | Illustration |
| ------- | ------------ |
| Higher iteration counts resulted in lower MAE values. **An iteration count of 200 was chosen.** | ![image](https://github.com/user-attachments/assets/392ac31c-95de-42c2-a9fe-cbf4033c76d0)|
| Lower minimum temperature values led to lower MAE values. Beyond 0.1, the MAE value remained steady. The maximum temperature range of 10 to 500 had a similar effect on the MAE. **A minimum temperature of 0.00001 and a maximum temperature of 100 were chosen.** |![image](https://github.com/user-attachments/assets/956a27ce-a300-4ec2-adc7-59e777c2e27a)|

**Decision Tree Regressor**
| Comment | Illustration |
| ------- | ------------ |
| Various subsets of the training data features were utilized, leading to different MAE values. The optimal feature set identified was ***r, theta, and phi.*** | ![image](https://github.com/user-attachments/assets/7d2d2af9-d1ef-4a88-987b-c87f4a491316)|
| The optimal maximum depth was found to be 20, with no further improvement observed beyond this depth. | ![image](https://github.com/user-attachments/assets/c38a35a3-c8cc-4e31-8749-613652e3913d) |
| A **maximum feature count of 3** was observed to yield lower MAE values. |![image](https://github.com/user-attachments/assets/c3e65c9c-97ae-41c3-843f-d5546f3d5cb1)|
| Increasing the minimum samples split raised the MAE value. **A value of 2 was selected.** | ![image](https://github.com/user-attachments/assets/bc502832-c64b-4caa-8d3b-8f8fd690d1ce)|

**Multi-layer Perceptron Regressor**
| Comment | Illustration |
| ------- | ------------ |
| After evaluating different hidden layer configurations, **a single hidden layer of 343 nodes was selected.** | ![image](https://github.com/user-attachments/assets/c5552c52-e5e1-4aec-b162-ab069f63487e)|
| A **ReLU activation function** was chosen for the hidden layers. | ![image](https://github.com/user-attachments/assets/bff182b5-3f53-48d9-9ffa-a33dd4c66dbe)|
| The **Adam solver** was selected for optimization. | ![image](https://github.com/user-attachments/assets/9d80ac97-9996-4711-88e5-63b2ad858339)|
| Minimal differences were noted across various learning rate strategies; therefore, a **constant learning rate** was used. | ![image](https://github.com/user-attachments/assets/6e361bbe-f1c3-413b-b586-f1e8fcf952a7)|
| An **initial learning rate of 0.001** was found to yield the smallest MAE value. |![image](https://github.com/user-attachments/assets/609c3358-13a8-4ef4-bdc4-b703e31cd701) |
| Smaller batch sizes contributed less to the MAE value, with **a batch size of 5 being selected.** | ![image](https://github.com/user-attachments/assets/53084e2a-1c17-4f38-bdb6-856b36302358)|


### Comparison of Posture and Position Errors

Upon determining the hyperparameter combinations that achieved satisfactory MAE results, these were employed to recommend the optimal configuration for each approach. This optimal setup was then utilized to predict the joint angles of the robotic arm for a randomly selected position from the test dataset. The predictions for each configuration are presented below, accompanied by a plot of the robot's posture where the red dot indicates the actual position.

Table 4: Comparison of Posture and Position Errors
| Name | MAE  in meters | Robot Posture |
| :---: | :---: | ----- |
| GA | 0.062 |![image](https://github.com/user-attachments/assets/a00df6ca-66a1-41ca-98ef-e9f2dcc5e540)|
| DE | 0.006 | ![image](https://github.com/user-attachments/assets/1ac33fb3-92dd-4a69-9b58-0bdd958f722b)|
| PSO | 0.0 | ![image](https://github.com/user-attachments/assets/9467dead-5549-49f9-a34d-0c45ae6d3445)|
| SA | 0.014 |![image](https://github.com/user-attachments/assets/f15cd1f5-7941-431d-a4f9-a611943c837c)|
| DT | 0.315 | ![image](https://github.com/user-attachments/assets/5b8a5949-4991-49df-9522-912c5b323c94)|
| MLP | 1.316 | ![image](https://github.com/user-attachments/assets/265d79bf-ac26-4f70-9636-122febb8d5d2)|

## CONCLUSION AND DISCUSSION
The Particle Swarm Optimization (PSO) strategy demonstrated superior performance, achieving a Mean Absolute Error (MAE) value approaching zero. This was followed by Differential Evolution (DE) with an MAE of 0.006, Simulated Annealing (SA) at 0.014, Genetic Algorithms (GA) with 0.062, Decision Tree (DT) at 0.315, and finally, Multi-layer Perceptron (MLP) with an MAE of 1.316. Despite the MLP exhibiting the highest MAE, it produced a smoother trajectory for the robot arm compared to the other methods. The PSO solution closely followed in terms of smoothness, whereas the SA solution resulted in the most angular path.

The primary objective of this project was to address the inverse kinematics problem for a 7-DOF robotic arm through the development and evaluation of various AI models and optimization techniques. This research explored the efficacy of different AI models, namely the Decision Tree Regressor and Multi-layer Perceptron Regressor, alongside optimization strategies like simulated annealing, particle swarm optimization, genetic algorithms, and differential evolution in mapping end-effector positions to corresponding joint configurations. Initially, the forward kinematics of the 7-DOF robotic arm was modelled, enabling the generation of a training dataset based on these principles. This dataset provided positions in Cartesian, polar, and spherical coordinate systems that aligned with different joint angle configurations. Subsequently, hyperparameter optimization was performed iteratively to identify the best values for each method. Ultimately, the optimal estimators from each category were compared based on the discrepancies between actual and predicted positions, confirming that the PSO model delivered the most accurate results with the least error.






