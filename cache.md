# Cache

## cache结构种类

### VIVT

`Virtual index Virtual Tag` : 使用虚拟地址索引域和虚拟地址的标记位

* 优势 :

  直接使用CPU发出的虚拟地址的索引域和标记域来查找`cache line`,而不用经过`MMU`的翻译,所以查找的速度是最快的

* 问题 :

  * `synonyms` : 多个不同的虚拟地址可能会被映射到同一个物理地址,即缓存别名系列

  * `homonyms` : 相同的虚拟地址指向不同的物理地址

    **这种情况下仅仅依靠`Virtual Index`无法区分这些相同的`cache`项**

    