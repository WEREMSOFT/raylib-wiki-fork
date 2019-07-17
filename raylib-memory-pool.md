# Memory Pool

By Kevin 'Assyrianic' Yonan @ https://github.com/assyrianic

**About**:
	The Raylib Memory Pool is a quick, efficient, and minimal free list & stack-based allocator.

**Purpose**:
	Raylib Memory Pool's purpose is the following list:
* A quicker, efficient memory allocator alternative to `malloc` and friends.
* Reduce the possibilities of memory leaks for beginner developers using Raylib.
* Being able to flexibly range check memory if necessary.

**Data Implementation**:
The memory pool encapsulates two public structs:
* `freeList` which is an abstracted doubly linked list consisting of a `head` and `tail` pointer to `MemNode`
* A `len` that tracks the amount of nodes the linked list holds.
* `maxNodes` which is used for auto-defragging (explained below), 
* and `autoDefrag` which controls whether auto-defragging will execute or not.
```c
	typedef struct MemNode {
		size_t size;
		struct MemNode *next, *prev;
	} MemNode;

	typedef struct AllocList {
		struct MemNode *head, *tail;
		size_t len, maxNodes;
		bool autoDefrag : 1;
	} AllocList;
```
	
`Stack` which is an array-based stack consisting of...
* a byte pointer to the entire array `mem`
* a base pointer `base` which changes position when allocating, deallocating, or defragging,
* and `size` which tracks how large the entire stack buffer is.
```c
	typedef struct Stack {
		uint8_t *mem, *base;
		size_t size;
	} Stack;
```
	
```c
	typedef struct MemPool {
		AllocList freeList;
		Stack stack;
	} MemPool;
```

**Usage**:
	The memory pool is designed to be used as a direct object.
We have two constructor functions:
```c
	MemPool MemPool_Create(size_t bytes);
	MemPool MemPool_FromBuffer(void *buf, size_t bytes);
```
To which you create a `MemPool` instance and give the function a max amount of memory you wish or require for your data.
Remember not to exceed that memory amount or the allocation functions of the allocator will give you a NULL pointer.
	
So we create a pool that will malloc 10K bytes.
```c
	MemPool pool = MemPool_Create(10000);
```
	
When we finish using the pool's memory, we clean it up by using `MemPool_Destroy`.
```c
	MemPool_Destroy(&pool);
```
	
Alternatively, if you're not in a position to use any kind of dynamic allocation from the operating system, you have the option to utilize an existing buffer as memory for the mempool:
```c
	char mem[64000];
	MemPool pool = MemPool_FromBuffer(mem, sizeof mem);
```
	
To allocate from the pool, we have two functions:
```c
	void *MemPool_Alloc(MemPool *mempool, size_t bytes);
	void *MemPool_Realloc(MemPool *mempool, void *ptr, size_t bytes);
```
	
`MemPool_Alloc` returns a (zeroed) pointer to a memory block.
```c
	// allocate an int pointer.
	int *i = MemPool_Alloc(&pool, sizeof *i);
```
	
`MemPool_Realloc` works similar but it takes an existing pointers and resizes its data, it does NOT zero the memory as it exists for resizing existing data. Please note that if you resize a smaller size, the data WILL BE TRUNCATED/CUT OFF.
If the `ptr` argument for `MemPool_Realloc`, it will work just like a call to `MemPool_Alloc`.
```c
	// allocate an int pointer.
	int *i = MemPool_Realloc(&pool, NULL, sizeof *i);
	
	// resize the pointer into an int array of 10 cells!
	i = MemPool_Realloc(&pool, i, sizeof *i * 10);
```
	
To deallocate memory back to the pool, there's also two functions:
```c
	void MemPool_Free(MemPool *mempool, void *ptr);
	void MemPool_CleanUp(MemPool *mempool, void **ptrref);
```
	
`MemPool_Free` will deallocate the pointer data back to the memory pool.
```c
	// i is now deallocated! Remember that i is NOT NULL!
	MemPool_Free(&pool, i);
```
	
`MemPool_CleanUp` instead takes a pointer to an allocated pointer and then calls `MemPool_Free` for that pointer and then sets it to NULL.
```c
	// deallocates i and sets the pointer to NULL.
	MemPool_CleanUp(&pool, (void **)&i);
	// i is now NULL.
```
	
Using `MemPool_CleanUp` is basically a shorthand way of doing this code:
```c
	// i is now deallocated! Remember that i is NOT NULL!
	MemPool_Free(&pool, i);
	
	// i is now NULL obviously.
	i = NULL;
```
	
	
Given that the memory pool is based on a stack allocator, that means to refill the stack, you must free in the opposite order you allocate memory. You can still free memory out of order but that'll create memory block fragments.
	
Freed memory blocks that are not absorbed into the stack allocator are placed into the allocator's free list. If you have too many nodes, the allocation functions might take a while to calculate as the allocating functions first check the free list for any memory blocks it can give you before falling back to the stack allocator.
	
If you have too many nodes (You can check by accessing the `freeList` struct and its `len` member like: `pool.freeList.len`), then you'll have to defragment the free list. Defragmenting consists of merging together memory blocks that are physically near one another.
	
You have two options in terms of defragging:
You can manually call the defrag function:
```c
	bool MemPool_DeFrag(MemPool *mempool);
```
... which will return true or false depending if it was able to merge any nodes.
	
Or you can set a node limit and enable auto defragging.
Auto defragging is disabled by default, you can turn it on or off either by calling:
```c
	void MemPool_ToggleAutoDefrag(MemPool *mempool);
```
or set it directly from the `freeList` struct member:
```c
	pool.freeList.autoDefrag = true; // set to on.
	pool.freeList.autoDefrag = false; // set to off.
	pool.freeList.autoDefrag ^= true; // toggle on or off.
```
	
For auto defragging to work, you must also set a maximum memory block node limit. You can set it directly with the `maxNodes` member in the `freeList` struct member:
```c
	// set free memory block node limit to 100!
	pool.freeList.maxNodes = 100UL;
```
If auto defragging is not enabled, then the node limit is ignored of course.
	
Finally, to get the total amount of memory remaining (to make sure you don't accidentally over-allocate), you utilize this function:
```c
	size_t MemPool_MemoryRemaining(const MemPool mempool);
```

# Object Pool
By Kevin 'Assyrianic' Yonan @ https://github.com/assyrianic

**About**:
The Raylib Object Pool is a fast and minimal fixed-size allocator.

**Purpose**:
Raylib Object Pool was created as a complement to the Raylib Memory Pool.
Due to the general purpose nature of Raylib Memory Pool, memory block fragmentations can affect allocation and deallocation speeds. Because of this, the Raylib Object pool succeeds by having no fragmentation and accommodating for allocating fixed-size data while the Raylib memory pool accommodates allocating variadic/differently sized data.

**Implementation**:
	The object pool is implemented as a hybrid array-stack of cells that are large enough to hold the size of your data at initialization:
```c
	typedef struct ObjPool {
		Stack stack;
		size_t objSize, freeBlocks;
	} ObjPool;
```

**Usage**:
	The object pool is designed to be used as a direct object.
We have two constructor functions:
```c
	ObjPool ObjPool_Create(size_t objsize, size_t len);
	ObjPool ObjPool_FromBuffer(void *buf, size_t objsize, size_t len);
```
	
To which you create a `ObjPool` instance and give the size of your object and how many objects for the pool to hold.
So assume we have a vector struct like:
```c
	typedef struct vec3D {
		float x,y,z;
	} vec3D_t;
```
... which will have a size of 12 bytes.
	
Now let's create a pool of 3D vectors that holds about 100 3D vectors.
```c
	ObjPool vector_pool = ObjPool_Create(sizeof(vec3D_t), 100);
```
	
Alternatively, if for any reason that you cannot use dynamic memory allocation, you have the option of using an existing buffer for the object pool:
```c
	vec3D_t vectors[100];
	ObjPool vector_pool = ObjPool_FromBuffer(vectors, sizeof(vec3D_t), 1[&vector] - 0[&vector]);
```
The buffer MUST be aligned to the size of `size_t` AND the object size must not be smaller than a `size_t` either.
	

Next, we start our operations by allocating which will always allocate ONE object...
If you need to allocate something like an array of these objects, then you'll have to make an object pool for the array of objects or use Raylib Memory Pool.

Allocation is very simple nonetheless!
```c
	vec3D_t *origin = ObjPool_Alloc(&vector_pool);
	origin->x = -0.5f;
	origin->y = +0.5f;
	origin->z = 0.f;
```
	
Deallocation itself is also very simple. There's two deallocation functions available:
```c
	void ObjPool_Free(ObjPool *objpool, void *ptr);
	void ObjPool_CleanUp(ObjPool *objpool, void **ptrref);
```
	
`ObjPool_Free` will deallocate the object pointer data back to the memory pool.
```c
	ObjPool_Free(&vector_pool, origin);
```
	
Like Raylib memory pool, the Raylib object pool also comes with a convenient clean up function that takes a pointer to an allocated pointer, frees it, and sets the pointer to NULL for you!
```c
	ObjPool_CleanUp(&vector_pool, (void **)&origin);
```
	
Which of course is equivalent to:
```c
	ObjPool_Free(&vector_pool, origin), origin = NULL;
```
