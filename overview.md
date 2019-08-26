# Polymorphic Memory Resources
 (PMR)

---

### Overview

* namespace `std::pmr`
* `polymorphic_allocator`
* `polymorphic_memory_resource`s
* Containers

---

### Why A new Allocator?

`std::vector<T, std::allocator<T>>` <br/> is a different type from <br/> `std::vector<T, MyAlloc<T>>`

<small>(or any other `std` container)</small>

---

### Polymorphic Allocation

* New in C++17
* Type erasure for allocators 
* Virtual call to alloc/dealloc

```cpp
#include <memory_resource>
```

---

### `std::polymorphic_allocator<T>`

* Wrapper around a _memory resource_
* Allocates a pointer or contructs  object
* Move-only
* Convertable from any `T` to any `U`
* Comparable `==`, `!=`

---

### `std::memory_resource`

* Abstract base class
* Not templated
* Small interface
  * do_allocate, do_deallocate, is_equal


---

### `std::memory_resource` (2)

```cpp
class memory_resource {
  public:
    virtual memory_resouce() = default;
    void* allocate(std::size_t bytes, std::size_t alignment = /*...*/);
    void deallocate(void* p, std::size_t bytes, std::size_t alignment = /*...*/);
    bool is_equal(const memory_resource& other) const noexcept;
  private:
    virtual void* do_allocate(std::size_t bytes, std::size_t alignment) = 0;
    virtual void do_deallocate(void* p, std::size_t bytes, std::size_t alignment) = 0;
    virtual bool do_is_equal(const std::pmr::memory_resource& other) const noexcept = 0;
}
``` 
<!--  .element: class="strech" style="width: 100% -->

---

### predefined memory resources

* `new_delete_resource()` 
* `null_memory_resource()`
* `get_default_resource()`, `set_default_resource()`
* `synchronized_pool_resource`
* `unsynchronized_pool_resource`
* `monotonic_buffer_resource`

---

# Demo Time

 `demo1.cpp`

---

### Demo Time (a)

Now change to `std::pmr::vector<>`

---

###  Containers

* `std::pmr::` variant of `std` containers with allocators
* vector, list, deque, ...
* set, map, unordered_*, ...

```cpp
namespace std::pmr {
    template <typename T>
    using vector = ::std::vector<T, 
            ::std::pmr::polymorphic_allocator>;
}
```



---

# Demo Time (b)
 `demo1b.cpp`

---

### Automatic propagation

* std containers will automatically propagate allocators
* also std functions `allocate_shared()`
* see [`std::uses_allocator`](https://en.cppreference.com/w/cpp/memory/uses_allocator)
* simplified for user code with C++20 `std::make_obj_using_allocator`
