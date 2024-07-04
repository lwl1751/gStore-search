# -gStore-

基于gStore的金融数据库查询

中国科学院大学 图数据管理与分析课程作业

## 1. 数据库基本信息

简要函数定义如下：

```python
def sql(sparql) # 根据给定的 sparql 语句，在 gStore 中进行查询，获取对应的输出
def entity_exist(company) # 判断某一公司 company 是否存在于数据库中
```

该数据库包含 16,652 个实体，21,440 个三元组，每个三元组的主语是公司的名称，宾语是股东的名称。并获取了所有的实体列表。

以招商银行股份有限公司为例，每个节点具有属性如下：
| 属性名   | 属性值                                              |
|----------|-----------------------------------------------------|
| id       | file:///F:/d2r-server-0.7/holder8.nt#holder_copy/招商银行股份有限公司 |
| 名称     | file:///F:/d2r-server-0.7/holder8.nt#holder_copy/招商银行股份有限公司 |
| 节点类型 | 实体                                                |
|||

## 2.任务1: 查询两个公司之间的关联路径（n-hop）, n>1

简要函数定义如下：

```python
def query_2_hop(entity_1,entity_2) # 查询两个公司之间的关联路径，2-hop
def query_n_hop(entity_1, entity_2,n = 3) # 查询两个公司之间的关联路径，n-hop，要求 n > 1，默认 n = 3
def generate_color_gradient(num_colors) # 生成长度为 num_color_gradient 的列表，用于绘图
def format_as_graph(results) # 使用 graphviz 库，将查询结果绘制为图表显示
```

实现了对任意两个公司间的n-hop关联路径路径查询(n > 1)，且进行了公司名称是否合法的判断。以招商局轮船股份有限公司与招商银行股份有限公司的3-hop关联路径查询为例，查询结果为

<p align="center">
  <img src="https://raw.githubusercontent.com/lwl1751/Image_Hosting/main/img/n_hop_query_results.png" alt="n_hop_query_results" width="70%">
</p>

## 3.任务2:实现多层股权的穿透式查询，可以根据指定层数获得对应层级的股东

简要函数定义如下：

```python
def query_1_hop(entity) # 查询该公司 entity 对应的持股公司
def query_multi_holder(entity,n = 3) # 实现 n 层股权的穿透式查询,默认n = 3
def draw_tree(res) # 使用 graphviz 库，将查询结果绘制为树状图显示
```

实现了对任意公司的多层股东查询，并记录了每个公司对应的持股公司。需要注意的是，每个公司可能有多个持股公司，而某些持股公司也可能持有多个被投股公司。为了简化图表绘制，我进行了去重处理，这一操作并不影响询结果的完整性和准确性。以招商局轮船股份有限公司的3层持股股东查询为例，文本类型的输出为

    层级 0:
    None --> 招商局轮船股份有限公司
    层级 1:
    招商局轮船股份有限公司 --> 深圳市招融投资控股有限公司
    招商局轮船股份有限公司 --> 深圳市晏清投资发展有限公司
    招商局轮船股份有限公司 --> 兴业证券股份有限公司
    招商局轮船股份有限公司 --> 招商局资本投资有限责任公司
    层级 2:
    深圳市招融投资控股有限公司 --> 招商银行股份有限公司
    深圳市招融投资控股有限公司 --> 招商证券股份有限公司
    深圳市招融投资控股有限公司 --> 深圳市楚源投资发展有限公司
    兴业证券股份有限公司 --> 南方基金管理有限公司
    招商局资本投资有限责任公司 --> 招商局资本控股有限责任公司
    招商局资本投资有限责任公司 --> 招商局资本管理有限责任公司
    层级 3:
    招商银行股份有限公司 --> 招商基金管理有限公司
    招商证券股份有限公司 --> 东北证券股份有限公司
    招商证券股份有限公司 --> 中国南方航空股份有限公司
    招商证券股份有限公司 --> 大连冷冻机股份有限公司
    招商证券股份有限公司 --> 招商致远资本投资有限公司
    招商证券股份有限公司 --> 博时基金管理有限公司
    深圳市楚源投资发展有限公司 --> 深圳市集盛投资发展有限公司
    南方基金管理有限公司 --> 南方资本管理有限公司
    招商局资本控股有限责任公司 --> 国新国同（浙江）投资基金合伙企业（有限合伙）
    招商局资本控股有限责任公司 --> 赣州招商致远壹号股权投资合伙企业（有限合伙）
    招商局资本控股有限责任公司 --> 中新建招商股权投资有限公司
    招商局资本管理有限责任公司 --> 深圳市招商致远股权投资基金管理有限公司

图形化输出为：
![图片](https://raw.githubusercontent.com/lwl1751/Image_Hosting/main/img/multi_holder_tree.png)

## 4.任务3:实现环形持股查询，判断两家公司是否存在环形持股现象

简要函数定义如下：

```python
def check_circular_holdings(entity_A, entity_C,n = 10) # 查询 10-hop 内两家公司是否存在环形持股，借助任务2 的多层股东查询实现
def draw_circular_path(path_A_to_C, path_C_to_A) # 使用 graphviz 库，将查询结果绘制为图表显示
```

实现了任意两家公司在任意n-hop内的环形持股查询。注意的是，两家公司间可能存在多个环形持股现象，但代码搜索返回的只是其中一条环形持股路径。以招商证券股份有限公司到兖州煤业股份有限公司的环形持股查询为例，查询结果为

<p align="center">
  <img src="https://raw.githubusercontent.com/lwl1751/Image_Hosting/main/img/circular_holdings_path_2.png" alt="n_hop_query_results" width="50%">
</p>

以招商证券股份有限公司和兖州煤业股份有限公司的环形持股查询为例，由于只存在从招商局轮船股份有限公司到招商银行股份有限公司的单边路径，查询结果为

<p align="center">
  <img src="https://raw.githubusercontent.com/lwl1751/Image_Hosting/main/img/circular_holdings_path_1.png" alt="n_hop_query_results" width="40%">
</p>
