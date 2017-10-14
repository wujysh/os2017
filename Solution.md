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

- deal with kern/trap.c
in `trap_dispatch()`
In order to call the function `syscall()` in kern/syscall.c, we need five uint32_t parameters and the first syscallno parameter. 
Notice that the five types of system calls are defined in inc/syscall.h and they are all enum numbers. 
but i cannot find more things about system call in kern/trap.h which defines the trapframe.
The definition of the generic `syscall()` is found in lib/syscall.c from which we know the five parameters are in "DX, CX, BX, DI, SI" and the first parameter is in "AX". And they are the REGISTERS!!! (defined in inc/trap.h)
So all paramters of `syscall()` are clear.
As for the return type, notice that in lib/syscall.c `syscall()` the return value ret is associated with "a" which represents register eax in the assembly language. 
So it is very likely that the return type of syscall function is the the same as the first paramter. 
But the return type is int32_t. if ret>0 it will invoke a panic. 
So ret should be a non-positive number. 
Condidering these, i have the returned value assigned to register eax.

- deal with kern/syscall.c
in `syscall()`
we must call another `sysacll()` from lib/syscall.c. 

- look at lib/syscall.c
Below the generic syscall function, there are several specific functions that call the generic one to do specific jobs. And the first param they pass into syscall() is not their name(although they are the same), but the enum number defined in inc/syscall.h. 

- back in kern/syscall.c
So the solution is clear - depending on which syscallno we use, different specific syscall functions are called. That is the switch-case statement. 



