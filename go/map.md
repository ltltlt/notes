# map

## 拿到map的内部表示

```go
type hmap struct {
	// Note: the format of the Hmap is encoded in ../../cmd/internal/gc/reflect.go and
	// ../reflect/type.go. Don't change this structure without also changing that code!
	count     int // # live cells == size of map.  Must be first (used by len() builtin)
	flags     uint8
	B         uint8  // log_2 of # of buckets (can hold up to loadFactor * 2^B items)
	noverflow uint16 // approximate number of overflow buckets; see incrnoverflow for details
	hash0     uint32 // hash seed

	// bmap的数组, 有2^B个元素
	buckets    unsafe.Pointer // array of 2^B Buckets. may be nil if count==0.
	oldbuckets unsafe.Pointer // previous bucket array of half the size, non-nil only when growing
    nevacuate  uintptr        // progress counter for evacuation (buckets less than this have been evacuated)
    
    // mapextra
}

p := **(**hmap)(unsafe.Pointer(&mymap))
```

## 内部数据结构

其实和array+链表差不多, 不过链表每个条目指向一个bmap(可以看作一个大小为8多数组), 每个能存8个k, v; 一定程度上减小了内存分配和回收, 内存也更连续些了

## grow

如果overflow过多(过多k映射到一处)或达到load factor, 会grow

初始化就是设置将buckets设为新的数组(如果达到load factor则新数组长度为原先多二倍), 将oldbuckets设为原先多数组

每次assgin或delete时判断如果grow进行中, 就进行逐步迁移(growWork), 每次最多迁移两个bucket
