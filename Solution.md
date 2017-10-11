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
I create a new Env** to do the job. Is it OK?
`env_alloc()` will create a new environment and store it in the first parameter which is a Env**.
 set env_type to what? only one choice now.
