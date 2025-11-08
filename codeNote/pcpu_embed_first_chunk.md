从setup_per_cpu_areas这看到的


```c
struct pcpu_group_info {
	int			nr_units;	/* aligned # of units */				//有多少个单位
	unsigned long		base_offset;	/* base address offset */	//地址偏移
	unsigned int		*cpu_map;	/* unit->cpu map, empty			//这个不好理解
						 * entries contain NR_CPUS */
};
struct pcpu_alloc_info {		//总之是用来记录分配信息的		
	size_t			static_size;
	size_t			reserved_size;
	size_t			dyn_size;
	size_t			unit_size;
	size_t			atom_size;
	size_t			alloc_size;
	size_t			__ai_size;	/* internal, don't use */
	int			nr_groups;	/* 0 if grouping unnecessary */  	
	struct pcpu_group_info	groups[];								//每个组有啥用？
};
/ arch / x86 / kernel / setup_percpu.c
static int __init pcpu_cpu_to_node(int cpu)
{
	return early_cpu_to_node(cpu);
}
"./arch/x86/include/asm/topology.h"
    //这个主要是看x86_cpu_to_node_map_early_ptr有没有值 没有的话就取__per_cpu_offset的偏移
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function))
int early_cpu_to_node(int cpu)	
{
    return *((x86_cpu_to_node_map_early_ptr) ? 
        &(x86_cpu_to_node_map_early_ptr)[cpu] : 
        &(*({ do { 
                   const void __seg_gs* __vpp_verify = (typeof((&(x86_cpu_to_node_map)) + 0))((void*)0); 
                   (void)__vpp_verify; 
                } while (0); 
                ({ unsigned long __ptr; __asm__("" : "=r"(__ptr) : "0"((__typeof_unqual__(*((&(x86_cpu_to_node_map))))*)((unsigned long)((&(x86_cpu_to_node_map)))))); 
                (typeof((__typeof_unqual__(*((&(x86_cpu_to_node_map))))*)((unsigned long)((&(x86_cpu_to_node_map)))))) (__ptr + (((__per_cpu_offset[(cpu)])))); 		
                    }); 
            })));
}
int __init pcpu_embed_first_chunk(size_t reserved_size, size_t dyn_size,
				  size_t atom_size,
				  pcpu_fc_cpu_distance_fn_t cpu_distance_fn,
				  pcpu_fc_cpu_to_node_fn_t cpu_to_nd_fn)
{
	void *base = (void *)ULONG_MAX;		//先初始成最大化值
	void **areas = NULL;				//
	struct pcpu_alloc_info *ai;			//看来是准备存一些信息了
	size_t size_sum, areas_size;		//没什么说的
	unsigned long max_distance;			//距离是什么意思
	int group, i, highest_group, rc = 0;

	ai = pcpu_build_alloc_info(reserved_size, dyn_size, atom_size,
				   cpu_distance_fn);
	if (IS_ERR(ai))
		return PTR_ERR(ai);

	size_sum = ai->static_size + ai->reserved_size + ai->dyn_size;
	areas_size = PFN_ALIGN(ai->nr_groups * sizeof(void *));

	areas = memblock_alloc(areas_size, SMP_CACHE_BYTES);
	if (!areas) {
		rc = -ENOMEM;
		goto out_free;
	}

	/* allocate, copy and determine base address & max_distance */
	highest_group = 0;
	for (group = 0; group < ai->nr_groups; group++) {
		struct pcpu_group_info *gi = &ai->groups[group];
		unsigned int cpu = NR_CPUS;
		void *ptr;

		for (i = 0; i < gi->nr_units && cpu == NR_CPUS; i++)
			cpu = gi->cpu_map[i];
		BUG_ON(cpu == NR_CPUS);

		/* allocate space for the whole group */
		ptr = pcpu_fc_alloc(cpu, gi->nr_units * ai->unit_size, atom_size, cpu_to_nd_fn);
		if (!ptr) {
			rc = -ENOMEM;
			goto out_free_areas;
		}
		/* kmemleak tracks the percpu allocations separately */
		kmemleak_ignore_phys(__pa(ptr));
		areas[group] = ptr;

		base = min(ptr, base);
		if (ptr > areas[highest_group])
			highest_group = group;
	}
	max_distance = areas[highest_group] - base;
	max_distance += ai->unit_size * ai->groups[highest_group].nr_units;

	/* warn if maximum distance is further than 75% of vmalloc space */
	if (max_distance > VMALLOC_TOTAL * 3 / 4) {
		pr_warn("max_distance=0x%lx too large for vmalloc space 0x%lx\n",
				max_distance, VMALLOC_TOTAL);
#ifdef CONFIG_NEED_PER_CPU_PAGE_FIRST_CHUNK
		/* and fail if we have fallback */
		rc = -EINVAL;
		goto out_free_areas;
#endif
	}

	/*
	 * Copy data and free unused parts.  This should happen after all
	 * allocations are complete; otherwise, we may end up with
	 * overlapping groups.
	 */
	for (group = 0; group < ai->nr_groups; group++) {
		struct pcpu_group_info *gi = &ai->groups[group];
		void *ptr = areas[group];

		for (i = 0; i < gi->nr_units; i++, ptr += ai->unit_size) {
			if (gi->cpu_map[i] == NR_CPUS) {
				/* unused unit, free whole */
				pcpu_fc_free(ptr, ai->unit_size);
				continue;
			}
			/* copy and return the unused part */
			memcpy(ptr, __per_cpu_start, ai->static_size);
			pcpu_fc_free(ptr + size_sum, ai->unit_size - size_sum);
		}
	}

	/* base address is now known, determine group base offsets */
	for (group = 0; group < ai->nr_groups; group++) {
		ai->groups[group].base_offset = areas[group] - base;
	}

	pr_info("Embedded %zu pages/cpu s%zu r%zu d%zu u%zu\n",
		PFN_DOWN(size_sum), ai->static_size, ai->reserved_size,
		ai->dyn_size, ai->unit_size);

	pcpu_setup_first_chunk(ai, base);
	goto out_free;

out_free_areas:
	for (group = 0; group < ai->nr_groups; group++)
		if (areas[group])
			pcpu_fc_free(areas[group],
				ai->groups[group].nr_units * ai->unit_size);
out_free:
	pcpu_free_alloc_info(ai);
	if (areas)
		memblock_free(areas, areas_size);
	return rc;
}
```

