# Report
## 1
>An explanation of your approach and design choices to help us understand how your particular implementation works.

## 2
### a)

Formula 5 from secrtion 2 is implemented with the following code:

```python 
dz_l = (-2 * (y_true - y_pred)) * model.df[l](model.z[l])
```
Formula 6 from secrtion 2 is implemented with the following code:

```python
dz_l = torch.mm(dz_prev, model.fc[str(l+1)].weight.data) * model.df[l](model.z[l])
```
Formula 4 from secrtion 2 is implemented with the following code:
```python
dw_l = (1/batch_size) * torch.mm(dz_l.t(), model.a[l-1]) 
db_l = (1/batch_size) * torch.sum(dz_l, dim=0, keepdims=True) 
db_l = torch.flatten(db_l)
```

With a pure pytorch implementation we would use `loss.backward()` and `optimizer.step()`  

### b)
> Cite a method used to check whether the computed gradient of a function seems correct. Briefly explain how you would use this method to check your computed gradients in section 2.


### c) 
Using PyTorch we specify the learning rate when we initialize the optimizer. 
``` python
optimizer = optim.SGD(model.parameters(), lr=0.01)
```


### d) 
Momentum is an extension to the gradient descent optimization algorithm that allows the search to build inertia in a direction in the search space and overcome the oscillations of noisy gradients and cost across flat sports of the search space. It is designed to accelerate the optimization process. One problem with the gradient descent algorithm is that the progression of the search can bounce around the search space based on the gradient. This can really slow down the process of the search, especially for those optimization problems where the broader trend or shape of the search space is more useful than specific gradients along the way. Momentum involves adding an additional hyperparameter that controls the amount of history (momentum) to include in the update equation. The value of a hyperparameter is defined in the range 0.0 to 0.1 and often has a value close to 1.0, i.e. 0.9. A momentum of 0.0 is the same as gradient descent without momentum.

### e)
Regularization is a technique used to reduce the errors by fitting the function appropriately on the given training set and avoid overfitting. The commonly used regularization techniques are L1 regularization, L2 regularization and Dropout regularization. In our code we use L2 regularization and the L2 is also called Ridge Regression. L2 uses "squared magnitude" of coefficient as penalty term of the loss function. 

### f)
> Report the different parameters used in section 3, question 8., the selected parameters in question 9. as well as the evaluation of your selected model.

#### Global
batch_size =  256
n_epoch =  30
loss_fn =  nn.CrossEntropyLoss()
seed =  265

#### Model 1
lr=0.01
weight_decay=0
momentum=0

Accuracy train: 0.88
Accuracy val: 0.83

#### Model 2
lr=0.01
weight_decay=0.01
momentum=0

Accuracy train: 0.86
Accuracy val: 0.83

#### Model 3
lr=0.01
weight_decay=0
momentum=0.9

Accuracy train: 0.90
Accuracy val: 0.81

#### Model 4
lr=0.01
weight_decay=0.001
momentum=0.9

Accuracy train: 0.96
Accuracy val: 0.84
**Accuracy test: 0.85**

#### Model 5
lr=0.02
weight_decay=0.01
momentum=0.8

Accuracy train: 0.88
Accuracy val: 0.81


### g)
We get expected results from our model with almos the same accuracy for validation (84%) and test (85%). This means we can conclude that our model will generalize well, by correctly labeling 85% og unseen images of birds or planes. 
