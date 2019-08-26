# My Problem
 

---

* Process memory usage increases over weeks
* Receives data from multiple sockets, processes, exports over sockets
* ~40 Threads active
* ~4-5 GiB of data in process concurrently
* Very many allocations/deallocations, containers and small objects


---

### Idea 1: profile

* Try valgrind/massif 
* Try gperftools/tcmalloc heapprofile 
* to slow to see any results
* \>12h for some minutes of playback data

:-(

---

### Idea2 2: counting_memory_resouce

* count memory allocations, deallocations, usage
* switch to pmr containers and allocate_shared
* search for possible culprits

---

# Demo Time (2)
 `demo2.cpp`

---

### Notes

* Simple to add statistics
* Memory Pools help
  * avoid new/delete calls
  * increase data locality
  * but do not release memory until the pool is destroyed

---

### Results for my Problem

* One `counting_memory_resouce` per software Component
  * see which component are most _active_
* And one per major data type
  * identify which data uses most memory
* found several 100MiB unused data structures
* memory pools seem to reduce memory fragmentation
