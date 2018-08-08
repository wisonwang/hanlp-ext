# elasticsearch-hanlp

当前支持分词类型：

- hanlp / hanlp-standard: 标准分词
- hanlp-index: 索引分词

## 编译、安装、测试

文件路径按着自己的安装路径设置

1. 编译、打包插件

```
gradle -p es-plugin jar buildPluginZip
```
    
2. 使用命令安装插件

```
ES_HOME/bin/elasticsearch-plugin install file:///home/hldev/hldata/data/hanlp-ext/es-plugin/build/distributions/elasticsearch-hanlp-5.4.3.zip
```
    
3. 修改 `ES_HOME/config` 目录下的 `jvm.options` 文件添加一行（读取hanlp.properties配置文件需要）
    
```
-Djava.security.policy=file:///你的ES目录/plugins/elasticsearch-hanlp/plugin-security.policy
```
    
4. 最后修改ES/bin/elasticsearch.in.sh文件将 ES_CLASSPATH修改为
    
```
ES_CLASSPATH="$ES_HOME/lib/elasticsearch-5.4.3.jar:$ES_HOME/lib/*:$ES_HOME/plugins/elasticsearch-hanlp/"
```

5. 最后运行elasticsearch即可

目前集成了handlp 和 handlp-index 两个分析器，用户可以搭配不同的tokenizer来实现不同的分析器，如下：

```
 PUT video-index
{
  "settings": {
    "analysis": {
      "analyzer": {
        "hanlp-standard": {
          "type": "custom",
         
          "tokenizer": "hanlp-standard"
          
        }
        ,"hanlp-nlp": {
          "type": "custom",
         
          "tokenizer": "hanlp-nlp"
          
        },
        "hanlp-nshort": {
          "type": "custom",
         
          "tokenizer": "hanlp-nshort"
          
        } ,"hanlp-crf": {
          "type": "custom",
         
          "tokenizer": "hanlp-crf"
          
        }
        ,"hanlp-speed": {
          "type": "custom",
         
          "tokenizer": "hanlp-speed"
          
        }
      }
      
    }
  }
}

GET video-index/_analyze?pretty
{
  "analyzer" : "hanlp-nlp",
  "text" : ["银河系中的至尊霸主灭霸（乔什·布洛林饰）带着几个得力手下洗劫了全宇宙，只为了将所有的无限宝石镶嵌于金属手套上，这个手套可以将整个银河系毁灭于弹指间。为了拯救宇宙，托尼·斯塔克（小罗伯特·唐尼饰）和史蒂夫·罗杰斯（克里斯·埃文斯饰）需要摒弃前嫌，重组复仇者联盟，并与蜘蛛侠（汤姆·赫兰德饰）、奇异博士（本尼迪克特·康伯巴奇饰）、银河护卫队、黑豹（查德维克·博斯曼饰）以及瓦坎达人民的力量一同作战"]
}

```


测试方法：

分别使用以下两种方式测试分词效果：

```curl
GET /_analyze?pretty
{
  "analyzer" : "hanlp",
  "text" : ["重庆华龙网海数科技有限公司"]
}
```

*输出：*

```json
{
  "tokens": [
    {
      "token": "重庆",
      "start_offset": 0,
      "end_offset": 2,
      "type": "ns",
      "position": 0
    },
    {
      "token": "华龙网",
      "start_offset": 2,
      "end_offset": 5,
      "type": "nr",
      "position": 1
    },
    {
      "token": "海数",
      "start_offset": 5,
      "end_offset": 7,
      "type": "nr",
      "position": 2
    },
    {
      "token": "科技",
      "start_offset": 7,
      "end_offset": 9,
      "type": "n",
      "position": 3
    },
    {
      "token": "有限公司",
      "start_offset": 9,
      "end_offset": 13,
      "type": "nis",
      "position": 4
    }
  ]
}
```

```curl
    GET /_analyze?pretty
    {
      "text" : ["重庆华龙网海数科技有限公司"]
    }
```

*输出：*

```json
{
  "tokens": [
    {
      "token": "重",
      "start_offset": 0,
      "end_offset": 1,
      "type": "<IDEOGRAPHIC>",
      "position": 0
    },
    {
      "token": "庆",
      "start_offset": 1,
      "end_offset": 2,
      "type": "<IDEOGRAPHIC>",
      "position": 1
    },
    {
      "token": "华",
      "start_offset": 2,
      "end_offset": 3,
      "type": "<IDEOGRAPHIC>",
      "position": 2
    },
    {
      "token": "龙",
      "start_offset": 3,
      "end_offset": 4,
      "type": "<IDEOGRAPHIC>",
      "position": 3
    },
    {
      "token": "网",
      "start_offset": 4,
      "end_offset": 5,
      "type": "<IDEOGRAPHIC>",
      "position": 4
    },
    {
      "token": "海",
      "start_offset": 5,
      "end_offset": 6,
      "type": "<IDEOGRAPHIC>",
      "position": 5
    },
    {
      "token": "数",
      "start_offset": 6,
      "end_offset": 7,
      "type": "<IDEOGRAPHIC>",
      "position": 6
    },
    {
      "token": "科",
      "start_offset": 7,
      "end_offset": 8,
      "type": "<IDEOGRAPHIC>",
      "position": 7
    },
    {
      "token": "技",
      "start_offset": 8,
      "end_offset": 9,
      "type": "<IDEOGRAPHIC>",
      "position": 8
    },
    {
      "token": "有",
      "start_offset": 9,
      "end_offset": 10,
      "type": "<IDEOGRAPHIC>",
      "position": 9
    },
    {
      "token": "限",
      "start_offset": 10,
      "end_offset": 11,
      "type": "<IDEOGRAPHIC>",
      "position": 10
    },
    {
      "token": "公",
      "start_offset": 11,
      "end_offset": 12,
      "type": "<IDEOGRAPHIC>",
      "position": 11
    },
    {
      "token": "司",
      "start_offset": 12,
      "end_offset": 13,
      "type": "<IDEOGRAPHIC>",
      "position": 12
    }
  ]
}
```
