---
layout: post
title: "Unorder list"
tags: [dna, list, data structure]
comments: true
date: 2016-07-26 00:00:00
modified: 2016-07-26 00:00:00
---
<!--more-->

```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

__author__ = 'Fu Yangzhen'


# python的list类是一种无序列表,表中的元素只包含其相对其他元素的位置信息
# 节点（Node）是实现链表的基本模块，每个节点至少包括两个重要部分。
# 首先，包含节点自身的数据，称为“数据域”。其次，包括对下一个节点的“引用”
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


class UnorderList:
	def __init__(self):
		self.head = None

	def isEmpty(self):
		return self.head == None

	def append(self, num):
		temp = Node(num)
		temp.setNext(self.head)  # temp的next索引交给旧的head索引
		self.head = temp  # 再把新head索引指向temp

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
			if current.getData() == num:
				found = True
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

	def insert(self, index, num):
		count = 0
		previous = None
		current = self.head
		while count < index:
			count += 1
			previous = current
			current = current.getNext()
		if previous == None:
			self.append(num)
		else:
			temp = Node(num)
			temp.setNext(current)
			previous.setNext(temp)

	def index(self, num):
		count = 0
		current = self.head
		while current != None:
			if current.getData() == num:
				return count
			else:
				count += 1
				current = current.getNext()
		raise ValueError('%s is not in list' % num)

	def pop(self, index=-1):
		size = self.size()
		count = 0
		current = self.head
		previous = None
		if -size <= index < size:
			site = (size + index) if index < 0 else index
			while count < site:
				count += 1
				previous = current
				current = current.getNext()
			if previous == None:
				self.head = current.getNext()
			else:
				previous.setNext(current.getNext())
			return current.getData()
		else:
			raise IndexError('pop index out of range')


# 链表类不包括任何节点对象，相反，它只包括对列表第一个元素的引用。
mylist = UnorderList()

mylist.append(31)
mylist.append(77)
mylist.append(17)
mylist.append(93)
mylist.append(26)
mylist.append(93)

print(mylist.show_list())
print('size:', mylist.size())
print('search 93:', mylist.search(93))
print('search 100:', mylist.search(100))
print('=========================')
mylist.append(100)
print('append and search 100:', mylist.search(100))
print(mylist.show_list())
print('size:', mylist.size())
print('=========================')
mylist.remove(100)
print('remove 100:', mylist.show_list())
mylist.remove(31)
print('remove 31:', mylist.show_list())
# insert,index和pop留为练习
mylist.insert(2, 666)
print('insert(2, 666):', mylist.show_list())
mylist.insert(0, 233)
print('insert(0, 233)', mylist.show_list())
print('=========================')
print('pop(0):', mylist.pop(0))
print(mylist.show_list())
print('pop():', mylist.pop())
print(mylist.show_list())
print('=========================')
mylist.index(93)
print('index(93):', mylist.index(93))
mylist.index(17)
print('index(17):', mylist.index(17))

# [93, 26, 93, 17, 77, 31]
# size: 6
# search 93: True
# search 100: False
# =========================
# append and search 100: True
# [100, 93, 26, 93, 17, 77, 31]
# size: 7
# =========================
# remove 100: [93, 26, 93, 17, 77, 31]
# remove 31: [93, 26, 93, 17, 77]
# insert(2, 666): [93, 26, 666, 93, 17, 77]
# insert(0, 233) [233, 93, 26, 666, 93, 17, 77]
# =========================
# pop(0): 233
# [93, 26, 666, 93, 17, 77]
# pop(): 77
# [93, 26, 666, 93, 17]
# =========================
# index(93): 0
# index(17): 4
```