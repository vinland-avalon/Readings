# sync.Map
# Summary
The sync.map is as easy as normal map within native Golang. However, it is *thread-safe*. As for performance, it not only remains close to `sync mutex + map` for set, but also is best in get cases. It is mostly because of its double-layer map design.
# Reference
- [Golang for all](https://golangforall.com/en/post/sync-map.html)
- [blog](https://www.ququ123.top/2022/04/golang_sync_map_principle/)
# How it Works
## Data Structure
```go
type Map struct {
	mu sync.Mutex

	read atomic.Value
	dirty map[interface{}]*entry
	misses int
}

type readOnly struct {
	m       map[interface{}]*entry
	amended bool 
}
```
Or it can be visualized as follows.
![Golang_sync_Map](https://github.com/vinland-avalon/Readings/blob/main/images/Golang_sync_Map.png?raw=true)
- The `mu` is mutex to lock when operating `dirty` map. 
- The `read` is actually struct `readOnly`, it is atomic safe, so it will be changed with atomic.CompareAndSwap. It is lock-free.
- The `dirty` map will store some of data, mostly new ones. The data within will be upgraded to `read` if reach some conditions. Such condition is related to `misses`.
- The `readOnly` contains a `m` map for read-only purpose. It also contains `amend` of bool type to tell if the dirty is empty.
- The values of maps are pointers to `entry`. Such pointers have 3 kinds of status, namely nil, valid and expunged.
## Operations
```go
 package main
    
    import (
    	"fmt"
    	"sync"
    )
    
    func main()  {
    	var m sync.Map
    	// 1. set
    	m.Store("qcrao", 18)
    	m.Store("stefno", 20)
    
    	// 2. get
    	age, _ := m.Load("qcrao")
    	fmt.Println(age.(int))
    
    	// 3. iterate
    	m.Range(func(key, value interface{}) bool {
    		name := key.(string)
    		age := value.(int)
    		fmt.Println(name, age)
    		return true
    	})

    	// 4. delete
    	m.Delete("qcrao")
    	age, ok := m.Load("qcrao")
    	fmt.Println(age, ok)
    
    	// 5. load, if not exist, store
    	m.LoadOrStore("stefno", 100)
    	age, _ = m.Load("stefno")
    	fmt.Println(age)
    }
```