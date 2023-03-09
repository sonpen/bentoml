# bentoml
https://docs.bentoml.org/en/latest/tutorial.html

### 가상환경 생성 및 activate
```bash
pyenv install 3.8.13
pyenv virtualenv 3.8.13 py38
pyenv virtualenv py38 bentoml
pyenv activate bentoml
```

### dependency 설치
```bash
pip install bentoml
pip install scikit-learn
```

### Saving Model with BentoML
```python
# main.py
import bentoml

from sklearn import svm
from sklearn import datasets

# Load training data set
iris = datasets.load_iris()
X, y = iris.data, iris.target

# Train the model
clf = svm.SVC(gamma='scale')
clf.fit(X, y)

# Save model to the BentoML local model store
saved_model = bentoml.sklearn.save_model("iris_clf", clf)
print(f"Model saved: {saved_model}")

# Model saved: Model(tag="iris_clf:zy3dfgxzqkjrlgxi")
```

### Creating a Service
```python
# service.py
import bentoml

from sklearn import svm
from sklearn import datasets

# Load training data set
iris = datasets.load_iris()
X, y = iris.data, iris.target

# Train the model
clf = svm.SVC(gamma='scale')
clf.fit(X, y)

# Save model to the BentoML local model store
saved_model = bentoml.sklearn.save_model("iris_clf", clf)
print(f"Model saved: {saved_model}")

# Model saved: Model(tag="iris_clf:zy3dfgxzqkjrlgxi")
```
```bash
(bentoml) user@AL01762848 bentoml % bentoml serve service.py:svc --reload
2023-03-09T20:58:49+0900 [INFO] [cli] Prometheus metrics for HTTP BentoServer from "service.py:svc" can be accessed at http://localhost:3000/metrics.
2023-03-09T20:58:50+0900 [INFO] [cli] Starting development HTTP BentoServer from "service.py:svc" listening on http://0.0.0.0:3000 (Press CTRL+C to quit)
2023-03-09 20:58:50 circus[43278] [INFO] Loading the plugin...
2023-03-09 20:58:50 circus[43278] [INFO] Endpoint: 'tcp://127.0.0.1:57931'
2023-03-09 20:58:50 circus[43278] [INFO] Pub/sub: 'tcp://127.0.0.1:57932'
2023-03-09T20:58:50+0900 [INFO] [observer] Watching directories: ['/Users/user/bentoml', '/Users/user/bentoml/models']
```
```bash
(base) user@AL01762848 ~ % curl -X POST \
>    -H "content-type: application/json" \
>    --data "[[5.9, 3, 5.1, 1.8]]" \
>    http://127.0.0.1:3000/classify
[2]%
```
