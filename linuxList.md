# list_head

[list_head](https://elixir.bootlin.com/linux/v6.16.3/C/ident/list_head) 代表着双向有头结点循环链表  尾部节点和头结点都是它

相关操作的那些宏呢？在[list.h - include/linux/list.h - Linux source code v6.16.3 - Bootlin Elixir Cross Referencer](https://elixir.bootlin.com/linux/v6.16.3/source/include/linux/list.h#L302)中

[LIST_HEAD_INIT](https://elixir.bootlin.com/linux/v6.16.3/C/ident/LIST_HEAD_INIT) 初始化是把前后都初始化给自己  不是置为空

list_for_each这个宏就是遍历 list

[list_for_each_entry](https://elixir.bootlin.com/linux/v6.16.3/C/ident/list_for_each_entry) 这个才应该常用呢  

# hlist_head

[hlist_head](https://elixir.bootlin.com/linux/v6.16.3/C/ident/hlist_head) 

[hlist_node](https://elixir.bootlin.com/linux/v6.16.3/C/ident/hlist_node) 中的pprev存的是前一个节点的next成员地址  因为next成员是指针  所以他是指针的指针，通过它就可以操作前一个节点的下一中next成员指向谁了

验证了 跟我想的一样  就是这么用  唉 还是得看看大伙怎么想的

啥时候能自主探索不靠外界

hash表使用方法参考[fs](https://elixir.bootlin.com/linux/v6.16.3/source/fs) / [inode.c](https://elixir.bootlin.com/linux/v6.16.3/source/fs/inode.c) 中的inode_hashtable

# 总结

总结一下就是两个经典链表 一个双链表 一个相同hash值的表 （指相同hash值的元素组成的表 ）

真正hash表是一个  [hlist_head](https://elixir.bootlin.com/linux/v6.16.3/C/ident/hlist_head) 数组
