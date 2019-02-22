---
title: "ElasticSearch Note"
tags: [other]
date: 2018-05-10 00:00:00
comments: true
---  

## Score formula   

ES版本跟新迭代的很快，截止到今天（2018.7.10) master的版本为6.3，打分公式一直都在微调，当前版本每个doc的对query的得分计算公式如下（就是最普通的BM25），其中n为query的term和当前doc的term的交集的大小：  


$$
score_d=\sum_{t=0}^ntfNorm_t*idf_t
$$


idf计算公式如下，其中docCount为index中doc总数，docFreq为曾出现当前计算的term的doc频数：  


$$
idf=log(1+\frac{docCount-docFreq+0.5}{docFreq+0.5})
$$


tfNorm的计算公式如下，其中fieldLength为当前doc的term的个数，avgFieldLength为index内所有doc的平均term个数，freq为当前计算的term的频数，$k_1$和b为参数，其默认值分别为1.2和0.75：  


$$
tfNorm=\frac{freq*(k_1+1)}{freq+k_1*(1-b+b*fieldLength/avgFieldLength)}
$$

注意：ES 为了分布式index和search的性能，`docFreq` , `docCount` and `avgFieldLength`are computed per shard.   

<!--more-->

## query flow  



## Analyzer    

目前原生的ES并未包含对中文分词的支持，需要另行安装插件来支持，最常用的插件有两个：

```bash
sudo bin/elasticsearch-plugin install analysis-smartcn
sudo bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.3.0/elasticsearch-analysis-ik-6.3.0.zip
```

测试方式：

```shell
curl -X POST "localhost:9200/_analyze" -H 'Content-Type: application/json' -d'
{
  "analyzer": "smartcn", # or "ik_smart"(best) , "ik_max_word"
  "text":     "无线电法国别研究"
}'
# different, smartcn：
{
  "tokens": [
    {
      "token": "无线电",
      "start_offset": 0,
      "end_offset": 3,
      "type": "word",
      "position": 0
    },
    {
      "token": "法国",
      "start_offset": 3,
      "end_offset": 5,
      "type": "word",
      "position": 1
    },
    {
      "token": "别",
      "start_offset": 5,
      "end_offset": 6,
      "type": "word",
      "position": 2
    },
    {
      "token": "研究",
      "start_offset": 6,
      "end_offset": 8,
      "type": "word",
      "position": 3
    }
  ]
}
# ik_smart:
{
  "tokens": [
    {
      "token": "无线电",
      "start_offset": 0,
      "end_offset": 3,
      "type": "CN_WORD",
      "position": 0
    },
    {
      "token": "法",
      "start_offset": 3,
      "end_offset": 4,
      "type": "CN_CHAR",
      "position": 1
    },
    {
      "token": "国别",
      "start_offset": 4,
      "end_offset": 6,
      "type": "CN_WORD",
      "position": 2
    },
    {
      "token": "研究",
      "start_offset": 6,
      "end_offset": 8,
      "type": "CN_WORD",
      "position": 3
    }
  ]
}
```

目前（v6.2.4^）在settings中使用`stopwords_path`的**唯一方式**(index建立了即生效，无需重启ES）：  

```bash
# index settings&mappings init
curl -X PUT "localhost:9200/my_index" -H 'Content-Type: application/json' -d'
{
  "settings": {
    "analysis": {
      "analyzer": {
        "ik_analyzer": {
          "type": "custom",
          "tokenizer": "ik_smart",
          "filter": [
            "ik_stop"
          ]
        }
      },
      "filter": {
        "ik_stop": {
          "type": "stop",
          "stopwords_path": "analysis-ik/extra_stopword.dic"
        }
      }
    }
  },
  "mappings": {
    "_doc": {
      "properties": {
        "content": {
          "type": "text",
          "analyzer": "ik_analyzer",
          "search_analyzer": "ik_analyzer"
        }
      }
    }
  }
}'

# chect analyzer
curl -X POST "localhost:9200/my_index/_analyze" -H 'Content-Type: application/json' -d'
{
    "analyzer":"ik_analyzer",
    "text":"这个也是他的了"
}'

# index some test samples
curl -X POST "localhost:9200/my_index/_doc" -H 'Content-Type: application/json' -d'
{
    "content":"这个也是他的了"
}'
curl -X POST "localhost:9200/my_index/_doc" -H 'Content-Type: application/json' -d'
{
    "content":"笔"
}'

# test search effect
curl -X POST "localhost:9200/my_index/_doc/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "explain": true,
  "query": {
    "match": {
      "content": "这个也是他的笔了"
    }
  }
}'
```

如果更新index的analysis，需要先关闭index，然后更新，再打开：  

```bash
curl -X POST "localhost:9200/my_index/_close"
curl -X PUT "localhost:9200/my_index/_settings" -H 'Content-Type: application/json' -d'
{
  "settings": {
    "analysis": {
      "analyzer": {
        "ik_analyzer": {
          "type": "custom",
          "tokenizer": "ik_smart",
           #stopwords_path -> 写在这个位置是无效的
          "filter": [
            "ik_stop"
          ]
        }
      },
      "filter": {
        "ik_stop": {
          "type": "stop",
          "stopwords": [
            "的",
            "是"
          ]
        }
      }
    }
  }
}'
curl -X POST "localhost:9200/my_index/_open"
curl -X POST "localhost:9200/my_index/_analyze" -H 'Content-Type: application/json' -d'
{
  "analyzer":"ik_analyzer",
  "content" : "这个是无线电法国别研究"
}'
```



## Q  




$$
keywordsScore_d=10-editDis(queryKeywords,docKeywords)
$$



$$
embeddingScore_d=cosDis(queryEmbedding,docEmbedding)
$$



$$
featureScore_d=keywordsScore_d+embeddingScore_d
$$
