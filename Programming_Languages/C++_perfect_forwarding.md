# Perfect Forwarding
## Reference
- [medium](https://levelup.gitconnected.com/perfect-forwarding-647e1caaf879)
## Basic Concept
“Forwarding” is the process where one function forwards its parameter to another function. When it is perfect, the function should receive the same object passed from the function that does the forwarding.

In other words, perfect forwarding means we do not just forward objects, we also forward their salient properties, whether they are lvalues or rvalues, const or volatile.
## Pre-knowledge
- Right value
- Universial reference
- Template Reduction
## How to write a function to implement Perfect Forwarding
### 1. How should it look like
```C++
// Object, very simple, ignore is ok
class Object {
 public:
  Object() = default;

  void SetName(const std::string &name) { name_ = std::move(name); }
  std::string GetName() const { return name_; }

 private:
  std::string name_;
};

// overloaded functions
void UseObject(Object &) {
  std::cout << "calling UseObject(Object &)" << std::endl;
}

void UseObject(const Object &) {
  std::cout << "calling UseObject(const Object &)" << std::endl;
}

void UseObject(Object &&) {
  std::cout << "calling UseObject(Object &&)" << std::endl;
}

// main
int main() {
  Object object;
  const Object const_object;
  UseObject(object);
  UseObject(const_object);
  UseObject(std::move(object));
}
```
The result could be like:
- calling UseObject(Object &)
- calling UseObject(const Object &)
- calling UseObject(Object &&)

## 2. If forward directly, something goes wrong
```C++
template <typename T>
void NotForwardToUseObject(T x) {
  UseObject(x);
}

int main() {
  Object object;
  const Object const_object;
  NotForwardToUseObject(object);
  NotForwardToUseObject(const_object);
  NotForwardToUseObject(std::move(object));
}
```
Due to the `const` and `rvalueness` being ignored by the template deduction, it will result:
- calling UseObject(Object &)
- calling UseObject(Object &)
- calling UseObject(Object &)
## 3. Universal references, almost right
```C++
template <typename T>
void HalfForwardToUseObject(T &&x) {  // universal reference
  UseObject(x);
}
```
Only universal reference parameters encode information about the lvalueness and rvalueness of the arguments that are passed to them. The result will be:
- calling UseObject(Object &)
- calling UseObject(const Object &)
- calling UseObject(Object &)
## 4. Final revise with static_cast
```C++
template <typename T>
void ForwardToUseObject(T &&x) {
  UseObject(static_cast<T &&>(x));
}
```
## Or use C++ `<utility>` library
```C++
template <typename T>
void PerfectForwardToUseObject(T &&x) {
  UseObject(std::forward<T>(x));
}
```