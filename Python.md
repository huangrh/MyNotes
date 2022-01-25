1. [SHAP (SHapley Additive exPlanations)](https://github.com/helenaEH/SHAP_tutorial)

2. [SHAP Values Explained ](https://towardsdatascience.com/shap-explained-the-way-i-wish-someone-explained-it-to-me-ab81cc69ef30#:~:text=In%20a%20nutshell%2C%20SHAP%20values,answer%20the%20%E2%80%9Chow%20much%E2%80%9D.)

3. [SHAP library for Python](https://github.com/slundberg/shap)  
```
Looks like Google Colab needs shap.initjs() in every cell where there is a visualization.
```

https://towardsdatascience.com/a-novel-approach-to-feature-importance-shapley-additive-explanations-d18af30fc21b

https://www.kaggle.com/dansbecker/advanced-uses-of-shap-values  

https://stackoverflow.com/questions/60311847/what-is-the-expected-value-field-of-treeexplainer-for-a-random-forest
```
from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.ensemble import GradientBoostingRegressor, RandomForestRegressor
import numpy as np

import shap
shap.__version__
# 0.37.0

X, y = make_regression(n_samples=1000, n_features=10, random_state=0)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

gbt = GradientBoostingRegressor(random_state=0)
gbt.fit(X_train, y_train)

mean_pred_gbt = np.mean(gbt.predict(X_train))
mean_pred_gbt
# -11.534353657511172

gbt_explainer = shap.TreeExplainer(gbt)
gbt_explainer.expected_value
# array([-11.53435366])

np.isclose(mean_pred_gbt, gbt_explainer.expected_value)
# array([ True])
```

# https://github.com/pwwang/datar
```
from datar import f
from datar.dplyr import mutate, filter, if_else
from datar.tibble import tibble
# or
# from datar.all import f, mutate, filter, if_else, tibble

df = tibble(
    x=range(4),
    y=['zero', 'one', 'two', 'three']
)
df >> mutate(z=f.x)
"""# output
        x        y       z
  <int64> <object> <int64>
0       0     zero       0
1       1      one       1
2       2      two       2
3       3    three       3
"""

df >> mutate(z=if_else(f.x>1, 1, 0))
"""# output:
        x        y       z
  <int64> <object> <int64>
0       0     zero       0
1       1      one       0
2       2      two       1
3       3    three       1
"""

df >> filter(f.x>1)
"""# output:
        x        y
  <int64> <object>
0       2      two
1       3    three
"""

df >> mutate(z=if_else(f.x>1, 1, 0)) >> filter(f.z==1)
"""# output:
        x        y       z
  <int64> <object> <int64>
0       2      two       1
1       3    three       1
"""
```