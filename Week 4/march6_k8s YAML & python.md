# Kubernetes YAML Files 

## namespace.yaml
Creates a **Namespace** called `k8s-demo`, which acts like a separate workspace in a cluster, preventing conflicts.

- Labels help categorize resources: `name: k8s-demo`, `environment: demo`, `app: k8s-master`.
- Keeps applications isolated and easier to manage.

## deployment.yaml
Defines a **Deployment** named `k8s-master-app` in the `k8s-demo` namespace. Ensures the correct number of app instances are always running.

- **Replicas:** `2` (two instances of the app)
- **Update Strategy:** `RollingUpdate`
- **Container Image:** `k8s-master-app:latest`
- **Exposed Port:** `5000`
- **Health Checks:** `/api/health`
- **Env Variables:** Loaded from **ConfigMaps** and **Secrets**.
- **Volumes:** Store config files, logs, etc.
- **Init Container:** Runs setup tasks before the main app starts.

## configmap.yaml
A **ConfigMap** (`app-config`) stores **non-sensitive** settings dynamically.

- Stores key-value pairs like `APP_NAME: "K8s Master App"`.
- Stores a JSON configuration (`app-settings.json`):
  ```json
  {"logLevel": "info", "enableMetrics": true, "maxFileSizeMB": 10}
  ```
- Allows easy config changes without redeploying the app.

## secret.yaml
A **Secret** (`app-secrets`) stores **sensitive** data securely.

- Stores Base64-encoded values like `SECRET_KEY: ZGV2LXNlY3JldC1rZXktMTIzNDU=`.
- More secure than ConfigMaps; ideal for passwords and API keys.
- Recommended to use **secret management tools** in production.

## service.yaml
A **Service** (`k8s-master-app`) ensures stable access to the deployment’s pods.

- **Type:** `NodePort` (external access via `<NodeIP>:30080`).
- **Selector:** `app: k8s-master` to find matching pods.
- **Ports:**
  - `port: 80` (internal service port)
  - `targetPort: 5000` (app port in pod)
  - `nodePort: 30080` (external access port)

## hpa.yaml
A **Horizontal Pod Autoscaler (HPA)** adjusts pod count based on CPU/memory usage.

- **Target:** `k8s-master-app`.
- **Min/Max Pods:** `1-5`.
- **Triggers:** Scales up if CPU or memory exceeds `50%`.
- **Delays:** Scale-up `60s`, scale-down `120s`.

## volumes.yaml
Defines a **ConfigMap (`app-files`)** to provide files as **volumes** to pods.

- Uses `emptyDir` for temporary storage.
- Stores files like `hello.txt`, `info.txt`, and `sample-config.txt`.
- **Production Tip:** Use **PersistentVolumes (PV)** instead.

---

### Object-Oriented Programming (OOP) in Python

Object-Oriented Programming (OOP) is a paradigm that organizes code into objects, which are instances of classes. Python, being an object-oriented language, allows developers to structure code using the four key principles of OOP:

#### 1. Encapsulation
Encapsulation is the concept of restricting direct access to some of an object's components and only allowing controlled interaction through methods. This is done using private (`_var` or `__var`) and public attributes.

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand  # Public attribute
        self.__model = model  # Private attribute
    
    def get_model(self):
        return self.__model  # Accessor method
```

#### 2. Inheritance
Inheritance allows a class (child class) to derive properties and behaviors from another class (parent class), promoting code reusability.

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand

    def show_brand(self):
        print(f"Brand: {self.brand}")

class Car(Vehicle):
    def __init__(self, brand, model):
        super().__init__(brand)
        self.model = model

    def show_details(self):
        print(f"Model: {self.model}")
```

#### 3. Polymorphism
Polymorphism enables different classes to define methods with the same name but different behaviors.

```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Bark"

class Cat(Animal):
    def speak(self):
        return "Meow"
```

#### 4. Abstraction
Abstraction hides the implementation details and only exposes the necessary functionality. This is achieved using abstract classes.

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def make_sound(self):
        pass

class Dog(Animal):
    def make_sound(self):
        return "Bark"
```

#### Method Overloading and Overriding
- **Method Overloading:** Python does not support traditional method overloading, but we can achieve it using default arguments or `*args` and `**kwargs`.

```python
class MathOperations:
    def add(self, a, b=0, c=0):
        return a + b + c
```

- **Method Overriding:** A child class can override a method defined in the parent class.

```python
class Parent:
    def show(self):
        print("This is the parent class")

class Child(Parent):
    def show(self):
        print("This is the child class")
```

### Conclusion
OOP in Python helps in structuring code efficiently by promoting modularity, reusability, and abstraction. By using OOP principles like encapsulation, inheritance, polymorphism, and abstraction, developers can build scalable and maintainable applications.

