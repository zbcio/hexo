---
title: ElasticSearch的一些操作
categories:
- 技术
- ElasticSearch
tags:
- ElasticSearch
---

### 1、update

POST http://es.zbc.io/{index}/{type}/{id}/_update

    {
      "doc": {
        "{column}": "{value}"
      }
    }

例如：  
POST http://es.zbc.io/logstash-car-ctrl/prpbusinesscontrol/

    B201800044820/_update
    {
      "doc": {
        "validstatus": "0"
      }
    }

参考：https://www.elastic.co/guide/cn/elasticsearch/guide/current/partial-updates.html

#### 2、insert

PUT http://es.zbc.io/{index}/{type}/{id}

    {
      "field": "value",
      ...
    }

例如：  
PUT http://es.zbc.io/logstash-car-ctrl/prpbusinesscontrol/B201800044820

    {
        "validstatus": "0"
    }


#### 3、创建.raw索引

PUT http://es.zbc.io/{index}

    {
      "mappings": {
        "{type}": {
          "properties": {
            "{column}": {
              "type": "string",
              "fields": {
                "raw": {
                  "type": "string",
                  "index": "not_analyzed"
                }
              }
            }
          }
        }
      }
    }

例如：  
POST http://es.zbc.io/logstash-car-ctrl

    {
      "mappings": {
        "prpbusinesscontrol": {
          "properties": {
            "engineno": {
              "type": "string",
              "fields": {
                "raw": {
                  "type": "string",
                  "index": "not_analyzed"
                }
              }
            }
          }
        }
      }
    }

#### 4、新增字段

POST http://es.zbc.io/{index}/{type}/_mapping

    {
      "properties": {
        "{column}": {
          "type": "string",
          "fields": {
            "raw": {
              "type": "string",
              "index": "not_analyzed"
            }
          }
        }
      }
    }

例如：  
POST http://es.zbc.io/logstash-car-ctrl/prpbusinesscontrol/_mapping

    {
      "properties": {
        "vinno2": {
          "type": "string",
          "fields": {
            "raw": {
              "type": "string",
              "index": "not_analyzed"
            }
          }
        }
      }
    }

