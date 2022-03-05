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
- 


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
