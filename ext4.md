[sd_init_command](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sd_init_command)   从这打断点就明了了

[iomap_dio_submit_bio](https://elixir.bootlin.com/linux/v6.16.3/C/ident/iomap_dio_submit_bio)  直觉告诉我 这个比较重要  好像不太是

[__iomap_dio_rw](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__iomap_dio_rw)

[ext4_file_read_iter](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_file_read_iter) 读的



AI告诉的

generic_file_read_iter

这是内核提供的通用函数，负责从页缓存中读取数据。如果数据不在缓存中，会触发缺页中断，将数据从磁盘加载到缓存。





