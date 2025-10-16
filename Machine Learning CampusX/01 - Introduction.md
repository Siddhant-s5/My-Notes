Types of Machine Learning
![[Pasted image 20250727085715.png]]
##### Supervised Learning
Here we have provided both input and output in dataset, we find the relation between those input & output and then based on that relation predict the output for new inputs.
Data can be of two types:
![[Pasted image 20250727090413.png]]
- **Regression**:- If supervised learning model has an output column of numerical data then the model is called Regression model.
- **Classification**:- Like regression if the output column is of categorical data then the model is called classification model. Here output is finite i.e. there can be a fixed number of categories are present (e.g. if output column is of Gender then there can be finite types of output i.e. Male or Female)
##### Unsupervised Learning
Here we only have the input and we need to analyse it using various methods like clustering, dimensnality reduction, etc to create the output.
##### Semi-supervised Learning
It is the method that uses a small amount of labeled data and a large amount of unlabeled data to train a model.
##### Reinforcement Learning
Here we don't provide any data, algorithm starts learning from scratch and gets improved by learning more and more like human. Reinforcement learning is an autonomous, self-teaching system that essentially learns by trial and error. It performs actions with the aim of maximizing rewards, i.e. it is learning by doing in order to achieve the best outcomes.
#### Batch Learning and Online Learning
- In **Batch learning**, the model is trained on entire available dataset and then that trained model is deployed to use. Because of this the deployed model never learns new things unless it is again trained on new data. To train model again we need to take it offline (as it needs to be trained over new available data) and then again train it and then again deploy it.
- In **Online learning**, the algorithm/model is trained on individual data instances or small batches of data as they become available. The model is updated continuously as new data arrives, without the need to re-train on the entire dataset. So, we can train our model everytime on the deployed state with the new data.
- In online learning, model forgots the old things(data) as it learns more and more new things(data), so a proper **learning rate** needs to be set.
- **Learning rate**: It shows how frequently we are training our model or providing new data to the model to learn.
##### There are two types for how machine learning model learns
1. **Instance Based Learning (Memorizing)**:-
   - Also known as memory-based learning or lazy learning
   - It involves storing the training instances and making predictions based on their similaritiy to new instances.
   - There's no explicit model building during training instead the entire training dataset is stored.
   - Predictions are made by comparing new instances to the stored instances using a similarity measure (e.g. Euclidean distances, cosine similarity).
   - e.g. K-Nearest Neighbours (K-NN)
2. **Model Based Learning**:-
   - Involves building a model from the training data, which captures the underlying patterns and relationships and form a mathematical equation so that it can make prediction using it.
   - The model is learned during the training phase and used to make predictions on new data.
   - e.g. regression, classification, clustering algorithms, etc.
  As we can see, instance based learning do nothing untill we give the input for the prediction, it just sits with prepared data we have given, while model based learning processes the data to train himself and then develop a mathematical model which it uses for the prediction for given input. So, *Model based learning* don't need training data after model is trained, whereas *Instance based learning* must keep training data as each query uses part/full set of training observations.
### Tensors
- Tensors are basically containers for 