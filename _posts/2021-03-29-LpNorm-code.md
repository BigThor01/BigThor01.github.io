# Lp Norm 코드

## Module import


```python
import matplotlib.pyplot as plt
import numpy as np
```

## p 에 따른 Lp Norm 등고선


```python
p = [0., 0.25, 0.5, 1., 1.5, 2., 5., np.inf]
xx, yy = np.meshgrid(np.linspace(-3,3, num = 101), np.linspace(-3,3, num = 101))

fig, axes = plt.subplots(ncols=(len(p) + 1)//2, nrows = 2, figsize = (14,7))

for p, ax in zip(p, axes.flat):
    if p == 0:
        zz = (xx != 0).astype(int) + (yy != 0).astype(int)
        ax.imshow(zz, cmap = 'bwr', extent = (xx.min(), xx.max(), yy.min(), yy.max()), aspect ="auto")
    else:
        if np.isinf(p):
            zz = np.maximum(np.abs(xx), np.abs(yy))
        else:
            zz = (np.abs(xx)**p + np.abs(yy)**p)**(1/p)
        
        ax.contourf(xx,yy,zz,30,cmap='bwr')
        ax.contour(xx,yy,zz,[1],colors='red',linewidths=2)
    ax.set_title('p = {}'.format(p))
plt.show()
```


![png](2021-03-29-LpNorm-code_files/2021-03-29-LpNorm-code_4_0.png)



```python

```
