---
title: "关于在data warehouse中实例化view的优化问题"
date: 2024-07-14T15:14:01+08:00
draft: false
---

## 问题背景

假设我们有一个名为SalesTable的SQL表格，这个表格有4列：time_、kind、shop和sales，分别代表某个时间段内某个种类的商品在某家商店的销售额。

假如我们需要统计每个时间段内每个种类商品的销售总额，那么我们可以这么编写SQL语句：
```sql
SELECT time_, kind, SUM(sales) FROM SalesTable GROUP BY time_, kind;
```

我们不妨将这个查询结果记作TK。

假如我们还需要统计每个种类商品的销售总额，那么我们可以这么编写SQL语句：

```sql
SELECT kind, SUM(sales) FROM SalesTable GROUP BY kind;
```

我们将这个查询结果记作K。要想得到K，还有另外一种方法，就是利用前面的TK：
```sql
SELECT kind, SUM(sales) FROM TK GROUP BY kind;
```

假如我们将TK的结果实例化（materialize，也就是缓存），那么就可以减少查询K的开销，因为TK的行数显然比SalesTable少得多。

## 问题定义

给定一个有向图$(G,E)$，图中的每个节点代表一个表格或者view，如果节点$B$可以由节点$A$得到，就建立一条边$A \to B$。我们可以实例化一些节点，把实例化的节点集合记作$M$。把一个节点的“邻居”和它自身记作$N(V)$，也就是说，$N(V)=\\{V\\} \cup \\{W| W \to V \in E\\}$。把一个节点的行数（数据规模）记作$S(V)$。一个节点的查询代价定义为
$$
C(V) = \min_{W\in N(V) \cap M} S(W)
$$

现在的问题是，我们已经实例化了一些节点（通常是根表格，比如说上面例子中的SalesTable），我们希望再实例化$K$个节点（$K$是一个常数），使得总查询代价$C=\sum_{V\in G}C(V)$能够减少最多。我们将这个问题的结果定义为这个**减少量**。

## 贪心算法

我们可以采用贪心算法解决这个问题，也就是说，我们依次选取节点加入$M$中，每次都选取一个能使$C$减少最多的节点。

Gupta [1] 证明了这么一个结论：贪心算法的近似比是$1-\frac{1}{e} \approx 0.63$。也就是说，贪心算法得到的结果至少是最优结果的0.63倍。

## 参考资料

[1] Gupta, H. (1997). Selection of views to materialize in a data warehouse. In Database Theory—ICDT'97: 6th International Conference Delphi, Greece, January 8–10, 1997 Proceedings 6 (pp. 98-112). Springer Berlin Heidelberg.