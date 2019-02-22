---
layout: post
title: "Order list"
tags: [dna, list, data structure]
comments: true
date: 2016-08-01 00:00:00
modified: 2016-08-01 00:00:00
---

>复现有序list小轮子,其中add方法应该把 寻找插入位置 和 插入Node 分开来,这样程序更加清晰,易读  
  
在有序列表中操作中，isEmpty和size仍然象无序表中一样，这是因为它们只关心数据项的个数，而不关心数据大小。  
同样的，remove的操作也完全一样，只涉及前后的节点，与数据大小无关。  
只有search和add方法，需要作出改变。 
<!--more--> 

```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

__author__ = 'Fu Yangzhen'


class Node:
	def __init__(self, initdata):
		self.data = initdata
		self.next = None

	def getData(self):
		return self.data

	def getNext(self):
		return self.next

	def setData(self, newdata):
		self.data = newdata

	def setNext(self, newnext):
		self.next = newnext


# 在有序列表中操作中，isEmpty和size仍然象无序表中一样，这是因为它们只关心数据项的个数，而不关心数据大小。
# 同样的，remove的操作也完全一样，只涉及前后的节点，与数据大小无关。
# 只有search和add方法，需要作出改变。
class OrderList:
	def __init__(self):
		self.head = None

	def isEmpty(self):
		return self.head == None

	def add(self, num):
		current = self.head
		previous = None

		while current != None:
			if current.getData() < num:
				previous = current
				current = current.getNext()
			else:
				break
				
		# add方法应该把 寻找插入位置 和 插入Node 分开来, 这样程序更加清晰, 易读
		temp = Node(num)
		if previous == None:
			temp.setNext(self.head)
			self.head = temp
		else:
			previous.setNext(temp)
			temp.setNext(current)

	def size(self):
		count = 0
		current = self.head
		while current != None:
			count += 1
			current = current.getNext()
		return count

	def search(self, num):
		found = False
		current = self.head
		while current != None:
			cur = current.getData()
			if cur == num:
				found = True
				break
			elif cur > num:
				break
			else:
				current = current.getNext()
		return found

	def remove(self, num):
		found = False
		current = self.head
		previous = None
		while current != None:
			if current.getData() == num:
				if previous == None:
					self.head = current.getNext()
				else:
					previous.setNext(current.getNext())
				break
			else:
				previous = current
				current = current.getNext()

	def show_list(self):
		l = []
		current = self.head
		while current != None:
			l.append(current.getData())
			current = current.getNext()
		return l

my_OrderList = OrderList()
my_OrderList.add(5)
print(my_OrderList.show_list())
my_OrderList.add(76)
print(my_OrderList.show_list())
my_OrderList.add(3)
print(my_OrderList.show_list())
my_OrderList.add(88)
print(my_OrderList.show_list())

print(my_OrderList.size())
print(my_OrderList.search(76))
print(my_OrderList.search(100))
```  
