# Solution
## Lab One

`Env` is a struct. `env_id` and `env_parant_id` is "typedef int_32" data type with special meaning of bits. 
`env_status` is an unsigned with five alternatives from an enmu.
`env_link` is the struct Env pointer that points to the next one. So it can be made into a link list. 

```
struct Env {
	struct Trapframe env_tf;	// Saved registers
	struct Env *env_link;		// Next free Env
	envid_t env_id;			// Unique environment identifier
	envid_t env_parent_id;		// env_id of this env's parent
	enum EnvType env_type;		// Indicates special system environments
	unsigned env_status;		// Status of the environment
	uint32_t env_runs;		// Number of times environment has run
	// Address space
	pde_t *env_pgdir;		// Kernel virtual address of page dir
};
```

- deal with kern/env.c
in `env_init()`
`envs` is a struct Env array. Mark its elements as is told in the comment. 
All elements need to be linked in `env_free_list` by linking them via a loop.
`env_alloc()` will assign `env_free_list` to `e` and returns it. So the first time it will make `e` the first element in the `env_free_list` - envs[0].

in `env_create()`
Q: I create a new Env** to do the job. Is it OK?
`env_alloc()` will create a new environment and store it in the first parameter which is a Env**.
 set env_type to what? only one choice now.

 in `env_run()`
 This function does context switch from curenv to e. 
 curenv is the old environment and e is the new one. 
 Call two functions to do sth. Follow the comment.
 Notice curenv can be NULL so it must be check before dereference.
 Q: what does "set the relevant parts of e->env_tf to sensible values" mean?
 env_tf is a complex struct called TrapFrame inside struct Env. 


- deal with kern/pmap.c
in `mem_init()`
line 160
Follow the codes above. 
`boot_alloc()` is the physical memory allocator only used for initialization. 
Use it like melloc function.

line 192
`boot_map_region()` used to map physical address to virtual address. 
Follow the case above and below. The comment is very similar.
Permission follows.
The "linear address" is given, `UENVS`. 
The physical address is gained by calling `PADDR()` with env as parameter. 
Q : The third parameter, size_t, should be the length of the array env ? 
