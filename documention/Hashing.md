## **散列**
散列表的实现常常叫做散列.
散列是一种用以常数平均时间执行插入,删除和查找的技术.
但是那些需要元素间任何排序信息的树操作(findMin,findMa...)将不会得到有效的支持.

1. **关键字**: 通常查找是项的某个部分(数据域)行,这部分叫关键字

2. **散列表**: 根据键值而直接访问数据的数组的数据结构

3. **散列函数**: 把每个关键字映射到[0,tableSize-1]的范围的一个映射(函数)
    - 策略: 
        - 不同键值可以映射到同一位置
        - 表的大小选择为素数
    - 实现
        - `键值为字符串`,把字符串中的ASCII码(或Unicode码)值加起来
            - 代码
        
                    public static int hash(String key,int tableSize){
                        int hashVal = 0;
                        for(int i = 0; i < key.length(); i++){
                            hashVal += key.charAt(i);
                        }
                        // hash的值的和对表大小取余
                        return hashVal % tableSize;
                    }
            - 缺点: 表很大时,因为ASCII码最大为127,字符串不是很长时,不能很好的均匀分配到表上
        - `前三个字母`
            - 代码
            
                    public static int hash(String key, int tableSize){
                        return (key.charAt(0) + 27 * key.charAt(1) + 729 * key.charAt(2) ) % tableSize;
                    }
            - 含义: 27为26个英文字母加一个空格的个数,分别取字符串前三位,再分别与27^n相乘,n为字符下标
            - 缺点: 因为单词中前英文字母不是随机的,某词典表示,3个字母的不同组合数实际只有2851.
        - `计算所有字母`
            - 代码
            
                    public static int hash(String key, int tableSize){
                        int hashVal = 0;
                        for(int i = 0; i < key.length(); i++){
                            hashVal = 37 * hashVal + key.charAt(i);
                        }
                        hashVal %= tableSize;
                        if(hashVal < 0){
                            hashVal += tableSize;
                        }
                        return hashVal;
                    }
4. **λ**: 散列表元素个数 / 散列表大小(即每个节点放多少个元素,即每个节点链表的长度)

5. **平均查找代价**: 1 + λ / 2(所以散列表的大小实际并不重

5. **冲突**
当一个元素被插入时与一个已经插入的元素散列到先相同的值,就会产生一个冲突.
    - 消除方法
        - `分离链接法`
            1. 定义: 将散列到同一个值的所有元素保留到一个表(链表)中
            2. λ: 尽量使该值约等于1
        - `相邻空间`(不使用额外表)
            1. 定义: 尝试另外一些单元,h0(x),h1(x),h2(x),...相继被尝试,hi(x) = (hash(x) + f(i)) mod TableSize,且f(0) = 0,函数f是冲突解决办法.
            2. 探测散列表: 对于不使用分离链接的散列表,其装填因子应该低于 λ = 0.5.
            3. 冲突解决方法:
                - `线性探测法`: 函数f是i的线性函数,典型情形是f(i)=i.
                    - 缺点: 随着元素增加,解决一次冲突(一次聚集)的时间越来越长,会逐渐填满整个表
                - `平方探测发`: 是冲突函数为二次的探测方法,流行的选择是f(i)=i^2
                    - 缺点: 对于填满整个表的情况更糟,因为若表大小不是`素数`,在表被填充一般之前,就不能保证一次找到空的单元;会造成`二次聚集`
                    - 特点: 表大小为`素数`,不能删除元素(因为冲突的元素需要用它找到)
                    - 再散列: 对于平方探测法,若表元素个数=1/2 * 表大小,则表示表已满,需要将散列表再扩大
                - `双散列`: f(i)=i*hash2(x),将第二个散列函数应用到x并在距离hash2(x),2hash2(x)...等处探测
                    - 散列函数: hash2(x)=R-(x mod R) 这样的函数将起到良好的作用,R为小于TableSize的素数.
                    - 特点: 模拟表明,预期的探测次数几乎和随机冲突解决方法的情形相同
                    
6. **再散列**
    - 定义: 若散列表填得太满,则操作的消耗时间变大,且可能失败,此时可通过建立另一个大约两倍大的表(而且使用一个相关的新散列函数),并重新插入原元素
    - 实现方式
        -  表满一半就再散列
        - 插入失败时再散列
        - 装填因子到达某个值时再散列(目前最好的策略)

7. **闪存散列代码**: String会缓存hash值留待下次使用,因为String是常量. 

## **完美散列**
   - 定义: 用足够多的散列表链接1级散列表,每个二级散列表将用一个不同的散列函数进行构造,知道没有冲突为止. 如果产生的冲突次数高于要求的值,主散列表也可以被构建多次.
   - 思路: 如果多一些表,则这些表的平均长度就会短一些,于是,理论上讲,如果我们有充分多的表,则在相当高概率下可以期待根本没有冲突
   - 注意: 如果所有的项都是事先知道,则完美散列是好用的
   - 定理: 
       - 若N个球被放入M=N^2个盒子,则没有任何盒子有超过1个球的概率不小1/2
       - 若N个项被放入包含N个盒子的主散列表中,则二级散列表的容量的期望值最多是2N
        
## **布谷鸟散列**
   - 定义: 假设有N个项,我们维护两个分别超过半空的表,且有两个独立的散列的函数,可以把每个项分配到每个表中的一个位置.布谷鸟散列保持不变的是一个项总是会被存储在这两个位置之一.
   - 定理: 
       - 如果N个项被随机地投入N个盒子,则盒子容量的最大期望值是Θ(log N/(log log N))
       - 有N个盒子,N个项,每次投放时,如果随机选取两个盒子,将该项投入(当时)比较空的那个盒子,则最大的盒子容量仅是Θ(log log N)
   - 现状发展: 布谷鸟散列表经常被实现成一张巨大的表,带有两个或多个可以探测整表的散列函数,以及各种变形算法,不是从一系列替换出发,而是只要有可用的地方就立刻尝试将一个项放入第二张散列表
    
## **跳房子散列(hopscotch hashing)**
   - 思路: 用事先确定的,对计算机的底层体系结构而言是最优的一个常数,给探测序列的最大长度加个上界. 
          这样可以给出常数级的最坏查询时间, 并且与布谷鸟散列一样,查询可以并行化,以同时检查可用位置的有限集.
   - 有效性: 跳房子散列对那些使用多处理器并且需要大量并行和并发的应用而言很有效.
  
## **通用散列法**
   1. 条件:
        - 散列函数必须可在常数时间(即与表中项的个数无关)内计算
        - 散列函数必须将各项均匀分布在数组单元中
   2. 定理:
        - 如果对于任意x≠y,H中有 h(x)=h(y) 的散列函数h的个数至多为|H|/M,则散列函数族H是通用的.
            - 适用散列: 分离链接法, 跳房子散列法
        - 如果对于任意x1≠y1,x2≠y2,...,xk≠yk,H中有 h(x)=h(y) 的散列函数h的个数至多为|H|/M^k,则散列函数族H是通用的.
            - 适用散列: 跳房子散列
        - 散列族H={Ha,b(x)=((ax+b)mod p)mod M, 其中1<=a<=p-1, 0<=b<=p-1}是通用的
        
## **可扩散列**
   1. 思路: 类似B树,随着M的增长,Bs树的深度降低. 理论上我们可以选择M非常大,使得B树的深度为1. 此时,在第一次以后的任何查找都将花费一次磁盘访问,因为根节点很可能存放在主存中.
            这种方法的问题在于分支系数太高,以至于为了确定数据在哪片树叶上要进行大量的处理工作.如果运行着一步的时间可以减缩,那我们就将有一个实际的方案.
   