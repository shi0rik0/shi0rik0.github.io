---
title: "在pybind11中操作numpy数组"
date: 2024-12-29T14:09:00+08:00
draft: false
---

最近需要给 Python 写一个涉及 numpy 数组操作的 C++扩展。我用的是`pybind11`，用这个库写 Python C++拓展非常方便，并且这个库也提供了对于 numpy API 的封装，可惜[官方文档](https://pybind11.readthedocs.io/en/stable/advanced/pycpp/numpy.html)写得比较晦涩，并且也不太全面。我结合官方文档和源代码，总结了一下 numpy 数组的操作方法。

## 头文件

首先要 include 以下头文件：

```cpp
#include <pybind11/buffer_info.h>
#include <pybind11/numpy.h>
#include <pybind11/pybind11.h>

namespace py = pybind11;
```

## numpy 数组在 pybind11 中的类型

在`pybind11`中，numpy 数组的类型是`py::array`。如果要限定数组的`dtype`，还可以用`py::array_t<T>`，其中`T`是数组元素的类型。例如，如果数组元素是`double`类型，那么数组的类型就是`py::array_t<double>`。

## 获取 numpy 数组的信息

获取 numpy 数组的信息可以使用`py::array_t`对象的`request()`方法，这个方法返回一个`py::buffer_info`对象，这个对象包含了数组的维度、元素类型、元素大小等信息。例如：

```cpp
void print_array_info(py::array arr) {
    py::buffer_info info = arr.request();
    std::cout << "ndim: " << info.ndim << std::endl;
    std::cout << "shape: ";
    for (auto s : info.shape) {
        std::cout << s << " ";
    }
    std::cout << std::endl;
    std::cout << "dtype: " << info.format << std::endl;
    std::cout << "itemsize: " << info.itemsize << std::endl;
}
```

假如我们这么调用这个函数：

```python
arr = np.zeros((3, 4), dtype=np.int32)
m.print_array_info(arr)
```

那么输出结果就是：

```
ndim: 2
shape: 3 4
dtype: l
itemsize: 4
```

## 访问、修改数组元素

如果只是访问数组元素，可以使用`unchecked()`方法，这个方法会返回一个 proxy 对象，通过这个对象可以直接访问数组元素。例如：

```cpp
void print_2d_array(py::array_t<double> arr) {
    if (arr.request().ndim != 2) {
        throw std::runtime_error("only 2D array is supported");
    }
    auto shape = arr.request().shape;
    auto proxy = arr.unchecked();
    for (int i = 0; i < shape[0]; i++) {
        for (int j = 0; j < shape[1]; j++) {
            std::cout << proxy(i, j) << " ";
        }
        std::cout << std::endl;
    }
}
```

如果还要修改元素，就需要使用`mutable_unchecked()`。例如：

```cpp
void add_one(py::array_t<double> arr) {
    if (arr.request().ndim != 1) {
        throw std::runtime_error("only 1D array is supported");
    }
    auto size = arr.request().shape[0];
    auto proxy = arr.mutable_unchecked();
    for (int i = 0; i < size; i++) {
        proxy(i) += 1;
    }
}
```

这里还要注意，`py::array_t`是一个引用类型，在传参过程中底层数据不会被复制，所以在函数内部修改数组元素会影响到原数组。

`unchecked()`和`mutable_unchecked()`方法都接受一个可选的模板参数，代表数组的维度。指定维度可以让编译器生成更高效的代码。例如：

```cpp
auto proxy = arr.unchecked<2>();
```

## 创建、返回 numpy 数组

如果要创建 numpy 数组，使用`py::array_t`的构造函数即可。其中一个构造函数接受数组的维度，然后返回指定维度的数组。要注意的是，返回值不会被初始化为全 0。这个构造函数的参数相当泛型，可以接受`std::vector`、`std::initializer_list`等类型。例如：

```cpp
py::array_t<double> create_array(int size) {
    return py::array_t<double>({size});
    // 因为是一维数组，所以也可以用 py::array_t<double>(size)
}
```

这个例子也展示了如何返回 numpy 数组，直接 return 即可。因为`py::array_t`是一个引用类型，所以返回的过程不会发生复制。
