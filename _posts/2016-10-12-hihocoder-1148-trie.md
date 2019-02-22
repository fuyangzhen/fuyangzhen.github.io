---
layout: post
title: "hihocoder 1148 — trie"
tags: [dna, hihocoder, oj]
comments: true
date: 2016-10-12 00:00:00
---

## 1 Trie树  
trie，又称前缀树或字典树。它利用字符串的公共前缀来节约存储空间。
性质:  

1. 根节点不包含字符，除根节点外的每个节点只包含一个字符。
2. 从根节点到某一个节点，路径上经过的字符连接起来，为该节点对应的字符串。
3. 每个节点的所有子节点包含的字符串不相同。  

应用:  

1. `词频统计`:比直接用hash节省空间
2. `搜索提示`:输入前缀的时候提示可以构成的词
3. `作为辅助结构`:如后缀树，AC自动机等的辅助结构  

<!--more-->
## 2 hihocode题目
以下题目来自：[hihocode](http://hihocoder.com/problemset/problem/1148)  
### 输入  
输入的第一行为一个正整数n，表示词典的大小，其后n行，每一行一个单词（不保证是英文单词，也有可能是火星文单词哦），单词由不超过10个的小写英文字母组成，可能存在相同的单词，此时应将其视作不同的单词。接下来的一行为一个正整数m，表示小Hi询问的次数，其后m行，每一行一个字符串，该字符串由不超过10个的小写英文字母组成，表示小Hi的一个询问。

在20%的数据中n, m<=10，词典的字母表大小<=2.

在60%的数据中n, m<=1000，词典的字母表大小<=5.

在100%的数据中n, m<=100000，词典的字母表大小<=26.

### 输出  
对于小Hi的每一个询问，输出一个整数Ans,表示词典中以小Hi给出的字符串为前缀的单词的个数。

### 样例输入  

```py
5
babaab
babbbaaaa
abba
aaaaabaa
babaababb
5
babb
baabaaa
bab
bb
bbabbaab
```  
### 样例输出  

```py
1
0
3
0
0
```  
## 算法实现  

```py
class TrieNode:
    def __init__(self):
        self.children = dict()
        self.isWord = False
        self.size = 0


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for letter in word:
            child = node.children.get(letter)
            if child is None:
                child = TrieNode()
                node.children[letter] = child
            node = child
            node.size += 1
        self.root.size += 1
        node.isWord = True

    def search(self, word):
        node = self.root
        for letter in word:
            node = node.children.get(letter)
            if node is None:
                return False
        return node.isWord

    def startsWith(self, prefix):
        node = self.root
        for letter in prefix:
            node = node.children.get(letter)
            if node is None:
                return False
        return True

    def cum_startsWith(self, prefix=''):
        if prefix == '':
            return self.root.size
        else:
            node = self.root
            for letter in prefix:
                node = node.children.get(letter)
                if node is None:
                    return 0
            return node.size
            
# hihocode 规定的测试输入输出结构：            
while True:
    try:
        m = int(raw_input())
        myTrie = Trie()
        for i in range(m):
            myTrie.insert(raw_input())
        n = int(raw_input())
        for i in range(n):
            print myTrie.cum_startsWith(raw_input())
    except EOFError:
        break
```  
## 4 测试  
```py
myTrie.insert('hello')
myTrie.insert('have')
myTrie.insert('z')
myTrie.insert('fun')
myTrie.insert('function')
myTrie.insert('func')
myTrie.insert('func')

print(myTrie.cum_startsWith('fun'))#4
print(myTrie.cum_startsWith('fu'))#4
print(myTrie.cum_startsWith('f'))#4
print(myTrie.cum_startsWith('z'))#1
print(myTrie.cum_startsWith('xx'))#0
print(myTrie.cum_startsWith())#7
print(myTrie.cum_startsWith(''))#7
print(myTrie.startsWith('x'))#False
print(myTrie.search('x'))#False
print(myTrie.startsWith('ha'))#True
print(myTrie.search('ha'))#False
```