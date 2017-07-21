linux 安装 greenplum loader
=========
kettle的表输出，插入更新控件对于greenplum的支持力度较小，在生产库中直接无法使用，建议使用kettle调用gp的load软件来处理数据；这样就需要在kettle服务器上装一套greenplum loader软件；或者将kettle服务器直接部署在gp的主节点上；建议采用第一种方案；避免对greenplum整个环境的影响。