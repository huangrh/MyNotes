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
