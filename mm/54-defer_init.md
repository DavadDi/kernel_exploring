defer_init���ں��е��ӳ�����

����һ�ڵ������С�page�ṹ�壬��Ҫ��ΰ��������꡷�����ǿ�����Ϊ��Ӧ�Ժ����ڴ棬�ں�����ΰ���page�ṹ��ġ�Ȼ������������黹û�н�����������Ӧ�Ժ����ڴ棬�ں���������һ�����ֵ����⡪��ʼ����

Page�ṹ����Ҫ��ʼ������ܼ��뵽buddy���������ں��и���ģ��ʹ�ã������֪�������ڴ����������ӣ���Ҫ��ʼ����page�ṹ��Ҳ�����ӡ����û�мǴ�Ļ������ڴ�ﵽT����page�ṹ���ʼ����ʱ�佫�ﵽ���Ӽ���

����ʲô�취�����������أ��еģ��Ǿ���

> �ӳ�����

��ʵ˼��ܼ򵥣���ϵͳ����ʱֻ��ʼ�����ֵ�page�ṹ�壬Ȼ��ʹ���ں��߳�����ʼ��ʣ�µ�page�ṹ�塣�Ӷ��ﵽ��������page�ṹ���ʼ����ϵͳ������Ӱ�졣

��˵���棬���ǻ���ֱ����������ɡ�

# �������

Ϊ����һ���Աȣ�������������û��defer_init�������

```
 start_kernel()
     setup_arch()
         x86_init.paging.pagetable_init() -> paging_init()
             sparse_init()
                 sparse_init_nid()
                     map = __populate_section_memmap()        (1)
                     sparse_init_one_section(sec, map)
             zone_sizes_init()
                 free_area_init() -> free_area_init_node()
                     free_area_init_core()
                         memmap_init() -> memmap_init_zone()
                             __init_single_page()             (2)
     mm_init() -> mem_init()
         memblock_free_all()
             free_low_memory_core_early()
                 __free_memory_core() -> __free_pages_memory()
                     memblock_free_pages()
                         __free_pages_core()                  (3)
```

Page�ṹ��Ҫ�����������̣������䣬����ʼ������ӵ�buddy��

����������Ƭ���У��ֱ�������������������������·�����ʱ������defer_initҪ����ľ���2,3�ڳ�ʼ��ʱռ�õ�ʱ����࣬����ϵͳ����ʱ�������

��Ȼ����2,3�ĵط�ռ����̫��ʱ�䣬��ô�Ͱ��ⲿ�ֵĹ����Ӻ�ִ�аѡ�

# �ӳ�����

���������������ں˴�������ΰ��ⲿ�ֵĹ����Ӻ�ִ�еġ�


```
 start_kernel()
     setup_arch()
         x86_init.paging.pagetable_init() -> paging_init()
             sparse_init()
                 sparse_init_nid()
                     map = __populate_section_memmap()
                     sparse_init_one_section(sec, map)
             zone_sizes_init()
                 free_area_init() -> free_area_init_node()
                     free_area_init_core()
                         memmap_init() -> memmap_init_zone()
                             defer_init()                     (1)
                             __init_single_page()
     mm_init() -> mem_init()
         memblock_free_all()
             free_low_memory_core_early()
                 __free_memory_core() -> __free_pages_memory()
                     memblock_free_pages()
                         early_page_uninitialised()           (2)
                         __free_pages_core()
     arch_call_rest_init() -> rest_init()
         pid = kernel_thread(kernel_init, NULL, CLONE_FS);
             kernel_init_freeable() -> page_alloc_init_late()
                 deferred_init_memmap()                       (3)
```

������Ĵ���Ƭ���п��Կ�������1��2�ĵط��ֱ��������жϣ��Ƿ�Ҫ�����ⲿ�ֵĳ�ʼ����

������3��λ��������һ���ں��߳�����ʼ��ʣ�µ�page�ṹ�塣

���ˣ�������������ô�򵥡�
