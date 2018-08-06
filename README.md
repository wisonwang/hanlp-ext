

# HanLP 扩展 #

## 说明 ## 

本项目为 https://github.com/yangbajing/hanlp-ext 项目的分支，因为原项目不支持最新的hanlp库，所以创建新的项目兼容之。

本次兼容的版本信息：
elasticsearch: 5.4.3
hanlp: 1.6.6

如果你需要更新的版本，可以尝试手动修改的依赖库版本，已经相应的配置

## 准备工作 ##

本项目依赖hanlp 项目，编译该工程需要从https://github.com/hankcs/HanLP/releases 下载最新的（1.6.6）版本的依赖jar包到到lib目录。

运行项目也需要依赖字典模型，可以从https://github.com/hankcs/HanLP 主页中找到训练好的模型字典。


## 模块说明 ##
- [es-plugin](es-plugin): HanLP Elasticsearch插件
- [hanlp-io](hanlp-io): HanLP IOAdapter，支持更多存储系统（这是一个示例项目，不应该编译加包，应参考里面的使用方式）

