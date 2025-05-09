# sklearn API
## 1. estimators
- fit
- prodict
- score
## 2. transformers 
### impute 
- from sklearn.impute import SimpleImputer
### preprocessing  
- from sklearn.preprocessing import OrdinalEncoder, OneHotEncoder, MinMaxScaler, StandardScaler, FunctionTransformer     
- ordinal_encoder.categories_
- cat_encoder.feature_names_in_
- cat_encoder.get_feature_names_out()

### Custom transformer  
- from sklearn.base import BAseEstimator, TransformerMixin
- from sklearn.utils.validation import check_array, check_is_fitted
- from sklearn.utils.
### tranforms API  
- fit
- transform
- fir_transform
## Predictors  
- predict
## Inspector 
- imputer.strategy
- imputer.statistics_

## 3. pipelines  
- from sklearn.compose import TransformedTargetRegressor, ColumnTransformer, make_column_selector, make_column_transformer
- from sklearn.pipeline import Piepeline, make_pipeline
- 
## 4. model_selecttion  
- from sklearn.model_selection import train_test_split
- from sklearn.model_selection import StratifiedShuyffleSplit
- from sklearn.model_selection import cross_val_score
- from sklearn.model_selection import cross_val_predict  
- from sklearn.model_selection import GridSearchCV
- from sklearn.model_selection import RandomizedSearchCV
- from sklearn.model_selection import StratifiedKFold
- 
## 5. metric   
- from sklearn.metrics.pairwise import rbf_kernel
- from sklearn.metrics import mean_squared_error
- from sklearn.metrics import confusion_matrix
- from sklearn.metrics import precision_score, recall_score, f1_score
- from sklearn.metrics import average_precision_score, precision_recall_curve
## 6. Model 
- from sklearn.linear_model import LinearRegression, SGDClassifier, 
- from sklearn.tree import DecisionTreeRegressor
- from sklearn.ensemble import RandomForestRegressor
- from sklearn.dummy import DummyClassifier
- from sklearn.ensemble import RandomForestClassifier  

## 7. Dataset 
- from sklearn.datasets import fetch_openml

## 8. other
- from sklearn.base import clone
- 

# kmeans: https://data36.com/k-means-clustering-scikit-learn-python/


[Multiclass and multioutput and multilabel](https://scikit-learn.org/stable/modules/multiclass.html)


https://towardsdatascience.com/understanding-the-confusion-matrix-from-scikit-learn-c51d88929c79

```
from sklearn import metrics
# Constants
C="Cat"
F="Fish"
H="Hen"

# True values
y_true = [C,C,C,C,C,C, F,F,F,F,F,F,F,F,F,F, H,H,H,H,H,H,H,H,H]
# Predicted values
y_pred = [C,C,C,C,H,F, C,C,C,C,C,C,H,H,F,F, C,C,C,H,H,H,H,H,H]

# Print the confusion matrix
print(metrics.confusion_matrix(y_true, y_pred))

# Print the precision and recall, among other metrics
print(metrics.classification_report(y_true, y_pred, digits=3))
```


# AUC Plot
```
def plot_auc(X,y,model):
    import matplotlib.pyplot as plt
    from sklearn import metrics
    pred = model.predict_proba(X)
    fpr, tpr, thresholds = metrics.roc_curve(y, pred[::,1], pos_label=1)
    auc = metrics.auc(fpr, tpr)
    plt.plot(fpr, tpr,label="ROC AUC="+str(round(auc, 3)))
    plt.xlabel("False Positive Rate")
    plt.ylabel("True Positive Rate")
    plt.legend(loc=4)
    plt.show()
```

# Model Eval: precision and recall, f1 score
```

def eval_perf(X,y,tops,model):
    
    from sklearn.metrics import confusion_matrix
    
    pred_test = model.predict_proba(X)
    p_sorted  = np.sort(pred_test[:,1])
    def mcm(top, p = p_sorted):
        cutoff = p[-1*top]
        y_pred_test = (pred_test[::,1] >=cutoff ).astype(int)
        # 
        tn, fp, fn, tp = confusion_matrix(y_true=y, y_pred = y_pred_test).ravel()
        total     = tn + fp + fn + tp
        top_pct   = top / total
        recall    = tp / (tp + fn)
        precision = tp/(tp + fp)
        f1        = 2/(1/precision + 1/recall)
        return top,top_pct, cutoff, tn, fp, fn, tp,total,precision, recall, f1

    res = list(map(mcm,tops))
    return pd.DataFrame(res, columns = ['top','top(%)','cutoff','tn', 'fp', 'fn', 'tp','total','precision', 'recall', 'f1 score'])

```


# XGBoost
```
import xgboost as xgb
xgb_cl = xgb.XGBClassifier(use_label_encoder=True,
                           learning_rate=0.1,
#                          num_iterations=1000,
                           max_depth=10,
                           eval_metric='mlogloss')
```

# Model Feature Importance

```
feat_importances = pd.Series(xgb_cl.feature_importances_, index=X_train.columns)
# 
feat_importances.nlargest(20).sort_values(ascending = True).\
plot(kind="barh",title="Feature Importance", figsize=(6,8))
plt.show()
```

# SHAP
```
# calculate shap value
import shap
explainer = shap.Explainer(model=xgb_cl)
shap_values = explainer(X_test)
# summarize the effects of all the features
shap.plots.beeswarm(shap_values)
# measure importance
shap.plots.bar(shap_values)
# visualize the first prediction's explanation
shap.plots.waterfall(shap_values[0])
```  
- https://towardsdatascience.com/explainable-ai-xai-with-shap-multi-class-classification-problem-64dd30f97cea  


- [SHAP (SHapley Additive exPlanations)](https://github.com/helenaEH/SHAP_tutorial)

- [SHAP Values Explained ](https://towardsdatascience.com/shap-explained-the-way-i-wish-someone-explained-it-to-me-ab81cc69ef30#:~:text=In%20a%20nutshell%2C%20SHAP%20values,answer%20the%20%E2%80%9Chow%20much%E2%80%9D.)

- [SHAP library for Python](https://github.com/slundberg/shap)  
```
Looks like Google Colab needs shap.initjs() in every cell where there is a visualization.
```

- https://towardsdatascience.com/a-novel-approach-to-feature-importance-shapley-additive-explanations-d18af30fc21b

- https://www.kaggle.com/dansbecker/advanced-uses-of-shap-values  

- https://stackoverflow.com/questions/60311847/what-is-the-expected-value-field-of-treeexplainer-for-a-random-forest  
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




# Save the model
```
# https://stackoverflow.com/questions/56107259/how-to-save-a-trained-model-by-scikit-learn
##########################
# SAVE-LOAD using joblib #
##########################
import joblib

# save
joblib.dump(clf, "model.pkl") 

# load
clf2 = joblib.load("model.pkl")

clf2.predict(X[0:1])
```

# https://neptune.ai/blog/f1-score-accuracy-roc-auc-pr-auc
