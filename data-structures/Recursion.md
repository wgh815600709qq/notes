### 递归简论

##### 递归的四条基本法则：
1. **基准情形**：必须总有某些基准情形，它无需递归就能解出
2. **不断推进**：对于那些需要递归求解的情形，每一次递归调用都必须要使求解状况朝接近基准情形的方向推进
3. **设计法则**：假设所有的递归调用都能运行
4. **合成效益法则**：在求解一个问题的同一实例时，切勿在不同的递归调用中做重复性的工作

##### 递归的问题
递归的问题是包含隐含的簿记开销。虽然这些开销几乎总是合理的（因为递归程序不仅简化了算法设计，而且也有助于给出更加简洁的代码），但是递归绝不应该作为简单 for 循环的代替物