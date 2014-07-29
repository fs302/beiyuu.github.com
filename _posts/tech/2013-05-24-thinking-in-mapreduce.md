---
layout: post
title: Thinking in MapReduce
category: tech
description: 不要以为你懂了WordCount了就学会了mapReduce]，更不用将如此General的框架想成只能做count操作的SB模型。
---

[![Map_reduce_overview](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/05/Map_reduce_overview.png)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/05/Map_reduce_overview.png)

MapReduce Overview

不要以为你懂了WordCount了就学会了[MapReduce](http://en.wikipedia.org/wiki/Mapreduce)，更不用将如此General的框架想成只能做count操作的SB模型。做完<Introduction to Data Science>的Programming Assignment3，才发现以前对MapReduce的理解有两个误区：
	
  * MapReduce只能做“数值统计”

  * 要做MapReduce，就必须自己写master，自己搭hadoop

其实，你只需要写好Mapper和Reducer，就可以直接跑MapReduce了。

简单回顾一下MapReduce的框架：

	【Map】
	input: (record)
	output: (intermediate key, value)
	【Shuffle】
	same_key -> Group
	【Reduce】
	input:Group of (intermediate key, value)
	output: (key, bag of values)

这样一个基于key-value的框架，能做些什么呢？

  1. 统计个数(objects count)：Map阶段，将输入数据分成一个个需要统计的object，Reduce阶段为每一个key做求和输出。具体的应用有对输入文本进行单词统计、社交网络朋友数统计等。【附例：wordcount.py】
	
  2. SQL Join查询：Map的输入为(table_name,raw_info)，将join的列名作为key，raw_info作为value生成键值对，在Reduce阶段对不同表，相同key的raw_info连接并输出。【附例：join.py】
	
  3. 矩阵乘法（PageRank！）：计算稀疏矩阵乘法A x B=C，输入为[matrix, i, j, value]表示矩阵matrix的i行j列的值为Value。对于A中的每个元素，用Mapper把它送至C中相同行的每一列记录中；对于B中的每个元素，用Mapper把它送至C中相同列的每一行记录中。然后在Reduce阶段作sigma(a(i,k)*b(k,j))的求和。【附例：MatrixMultiplication.py】


那么，以上这些操作，都有哪些共性呢？为什么可以用MapReduce，用它有什么好处呢？

通过分析我们可以发现，MapReduce应用的这些问题，都可以在Map阶段对原始数据做“**切分”**，并在Reduce阶段做**“求和”**，这个求和，不只是数值上的相加，实际上是对同组（group）的元素进行任意操作，包括字符串的连接、算术逻辑运算等。因为**可分**，使得大数据可以分散到集群中较弱的机器中处理，又因为每次**求和**只需要对同组元素进行操作，也大大降低了单机的压力。而且在Hadoop等MapReduce的实际架构中，有着很好的备份、容错能力，相比传统的关系型数据库甚至于并行数据库都有极大的优势（因为数据大、操作多，总有地方可能会down掉）。

最后，推荐一个在线学习实践和调试MapReduce的网站：JSmapreduce.com. 感谢Bill Howe和他的团队设计如此精彩的实践作业，有兴趣的同学赶紧加入进来吧：https://class.coursera.org/datasci-001/。

<!-- more -->附：

本地模拟MapReduce的框架代码：


	import json

	class MapReduce:
		def __init__(self):
			self.intermediate = {}
			self.result = []

		def emit_intermediate(self, key, value):
			self.intermediate.setdefault(key, [])
			self.intermediate[key].append(value)

		def emit(self, value):
			self.result.append(value)

		def execute(self, data, mapper, reducer):
			for line in data:
				record = json.loads(line)
				mapper(record)

			for key in self.intermediate:
				reducer(key, self.intermediate[key])

			#jenc = json.JSONEncoder(encoding='latin-1')
			jenc = json.JSONEncoder()
			for item in self.result:
				print jenc.encode(item)


其中，emit_intermediate函数在Map阶段被调用，用于存储(key,value)对，按key为索引建立列表字典。在wordcount中，就是发送(key,1)。emit函数是被Reduce调用，将整合好的(key,value)存储输出。如下图所示：


[![wordcount](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/05/wordcount.png)](http://www.wytk2008.net/wordpress/wp-content/uploads/2013/05/wordcount.png)

wordcount.py 示例：

> Create an Inverted index. Given a set of documents, an inverted index is a dictionary where each word is associated with a list of the document identifiers in which that word appears.

> **Mapper Input**
> The input is a 2 element list: [document_id, text]

> document_id: document identifier formatted as a string
> text: text of the document formatted as a string

> **Reducer Output**
> The output should be a (word, document ID list) tuple where word is a String and document ID list is a list of Strings.


	# Part 1
	mr = MapReduce.MapReduce()

	# Part 2
	def mapper(record):
		# key: document identifier
		# value: document contents
		key = record[0]
		value = record[1]
		words = value.split()
		for w in words:
		  mr.emit_intermediate(w, 1)

	# Part 3
	def reducer(key, list_of_values):
		# key: word
		# value: list of occurrence counts
		total = 0
		for v in list_of_values:
		  total += v
		mr.emit((key, total))

	# Part 4
	inputdata = open(sys.argv[1])
	mr.execute(inputdata, mapper, reducer)


Join.py

Implement a relational join as a MapReduce query

query:


> SELECT *

> FROM Orders, LineItem

> WHERE Order.order_id = LineItem.order_id



	import MapReduce
	import sys

	"""
	SQL Join in the Simple Python MapReduce Framework
	"""

	mr = MapReduce.MapReduce()

	# =============================
	# Do not modify above this line

	def mapper(record):
		# key: order_id
		# value: row_record
		key = record[1]
		value = [record[0],record]
		mr.emit_intermediate(key, value)

	def reducer(key, list_of_values):
		# key: order_id
		# value: raw_info
		for v in list_of_values:
			for u in list_of_values:
				if v[0]>u[0]:
					value = v[1]+u[1]
					mr.emit(value)

	# Do not modify below this line
	# =============================
	if __name__ == '__main__':
	  inputdata = open(sys.argv[1])
	  mr.execute(inputdata, mapper, reducer)


MatrixMultiplication.py


> Assume you have two matrices A and B in a sparse matrix format, where each record is of the form i, j, value. Design a MapReduce algorithm to compute matrix multiplication: A x B

> **Map Input**
> The input to the map function will be matrix row records formatted as lists. Each list will have the format [matrix, i, j, value] where matrix is a string and i, j, and value are integers.

> The first item, matrix, is a string that identifies which matrix the record originates from. This field has two possible values:

> ‘a’ indicates that the record is from matrix A

> ‘b’ indicates that the record is from matrix B

> **Reduce Output**
> The output from the reduce function will also be matrix row records formatted as tuples. Each tuple will have the format (i, j, value) where each element is an integer.


	import MapReduce
	import sys

	"""
	Matrix Multiplication in the Simple Python MapReduce Framework
	"""

	mr = MapReduce.MapReduce()

	# =============================
	# Do not modify above this line

	def mapper(record):
		col_max = 5 # max column number
		raw_max = 5 # max raw number
		matrix_id = record[0]
		row = record[1]
		col = record[2]
		value = record[3]
		if matrix_id=='a':
			for k in range(col_max):
				mr.emit_intermediate((row,k),(('a',row,col),value))
		if matrix_id=='b':
			for i in range(raw_max):
				mr.emit_intermediate((i,col),(('b',row,col),value))

	def reducer(key, list_of_values):
		M = 5
		i = key[0]
		k = key[1]
		sum = 0
		for term in list_of_values:
			if term[0][0]=='a':
				row = term[0][1]
				col = term[0][2]
				for ob in list_of_values:
					if ob[0][0]=='b' and ob[0][1]==col:
						sum += int(term[1])*int(ob[1])

		mr.emit((i,k,sum))

	# Do not modify below this line
	# =============================
	if __name__ == '__main__':
	  inputdata = open(sys.argv[1])
	  mr.execute(inputdata, mapper, reducer)

