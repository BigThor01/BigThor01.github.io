# Regularization 코드

## Module import


```python
import numpy as np
import matplotlib.pyplot as plt
```

## Data point 생성 함수 define


```python
def f(dat_size):
    '''
    @ Description
        @ y = 2 sin (1.5 x) 를 생성하는 함수
    @ Data
        @ Input : dat_size (generate 하는 point 수)
        @ Output : tuple (array x, array y)
    '''
    x = np.linspace(0, 8, dat_size)
    y = 2 * np.sin(1.5 * x)
    return (x,y)

def sample(dat_size):
    '''
    @ Description
        @ y = 2 sin (1.5 x) + epsilon 인 data point 를 생성하는 함수
    @ Data
        @ Input : dat_size (generate 하는 point 수)
        @ Output : tuple (array x, array y)
    '''
    x = np.linspace(0, 8, dat_size)
    y = 2 * np.sin(1.5 * x) + np.random.normal(size = x.size, scale = 2)
    return (x,y)
```

## Polynomial regression 함수 생성


```python
from sklearn.linear_model import LinearRegression

def fit_polynomial(x,y,degree):
    '''
    @ Description
        @ fit y ~ polynomial regression of x
    @ Data
        @ Input : x, y, degree
        @ Output : polynomial model
    '''
    model = LinearRegression()
    model.fit(np.vander(x, degree + 1),y)
    return model

def predict_polynomial(model, x):
    '''
    @ Description
        @ predict y by using model
    @ Data
        @ Input : model, x (new data)
        @ Output : predicted value y
    '''
    degree = model.coef_.size
    y = model.predict(np.vander(x, degree))
    return y
```

## Generate sample data


```python
np.random.seed(seed = 100)
# Underlying true model
f_x, f_y = f(100)
# Generate sample points
x, y = sample(15)
# Set figure size
plt.rcParams["figure.figsize"] = (10,5)

plt.plot(x, y, 'k.')
plt.plot(f_x, f_y, 'b-')
plt.show()
```


![png](2021-03-29-Regularization-code_files/2021-03-29-Regularization-code_8_0.png)


## Polynomial order 와 flexibility 확인


```python
plt.rcParams["figure.figsize"] = (15,10)

fig, axes = plt.subplots(2,2)
order_list = range(1, 15, 4)

for row in axes:
    for col in row:
        order = order_list[0]
        model = fit_polynomial(x, y, order)
        p_y = predict_polynomial(model, f_x)
        
        col.plot(x,y,'k.')
        col.plot(f_x, f_y, 'b-')
        col.plot(f_x, p_y, 'r-')
        col.set_title('{}-order polynomial'.format(order))
        
        order_list = order_list[1:]
```


![png](2021-03-29-Regularization-code_files/2021-03-29-Regularization-code_10_0.png)


## Penalized polynomial regression 함수 생성 (p = 2)


```python
from sklearn.linear_model import Ridge

def fit_polynomial_regularization(x, y, degree):
    '''
    @ Description
        @ fit y ~ polynomial regression of x with penalty term
    @ Data
        @ Input : x, y, degree
        @ Output : polynomial model with penalty term
    '''
    model = Ridge(alpha = 0.00001, normalize = True)
    model.fit(np.vander(x, degree + 1), y)
    return model
```

## Penalized polynomial 과 flexibility 확인


```python
fig, axes = plt.subplots(2,2)
order_list = range(1, 15, 4)

for row in axes:
    for col in row:
        order = order_list[0]
        model = fit_polynomial_regularization(x, y, order)
        p_y = predict_polynomial(model, f_x)
        
        col.plot(x,y,'k.')
        col.plot(f_x, f_y, 'b-')
        col.plot(f_x, p_y, 'r-')
        col.set_title('{}-order penalized polynomial'.format(order))
        
        order_list = order_list[1:]
```


![png](2021-03-29-Regularization-code_files/2021-03-29-Regularization-code_14_0.png)

