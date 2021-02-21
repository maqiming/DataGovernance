### Getting started
#### 流程
1. 需求分析
2. 拆分组件级需求
3. coding
4. 测试
5. 发布

#### 资源概览
1. 脚本概览: 定义函数时添加注释信息, 即可自动生成
    ```sql
        SELECT
            `subject` AS '计算层',
            cat AS '主题',
            module AS '模块',
            func AS '函数',
            doc AS '描述',
            path AS '路径',
            mtime AS '最近修改时间' 
        FROM
            rf_function
    ```

2. 数据资源:   

    ```sql
    SELECT
        `name` AS '名称',
        table_name AS '数据库表名',
        `desc` AS '描述',
        mtime AS '最近更新时间',
        job_name AS '对应job' 
    FROM
        rf_data
    ```
3. 任务概览和管理:

    ```sql
        SELECT
            job_name AS '任务名称',
            work_dir AS '脚本目录',
            module AS '模块名',
            func AS '入口函数名',
            priority AS '执行优先级',
            server_name AS '运算服务器',
            flag AS '执行批次',
            job_status AS '任务启用状态',
            start_time AS '开始时间',
            finish_time AS '完成时间',
            message AS '报错信息' 
        FROM
            rf_job
    ```
4. 执行日志:

    ```sql
        SELECT
            id,
            job_name AS '任务名称',
            start_time AS '开始时间',
            finish_time AS '完成时间',
            message AS '报错信息' 
        FROM
            rf_job_log
    ```

5. 配置模块```settings.py```

```python
TABLE_DATA = 'rf_data'
TABLE_FUNCTION = 'rf_function'
TABLE_JOB = 'rf_job'
TABLE_JOB_LOG = 'rf_job_log'
SERVER_NAME = 'centos_001'

DATA_DB = {
    'db_type': 'mysql',
    'username': '',
    'password': '',
    'host': '',
    'db_name': '',
    'port': 3306
}

CONFIG_DB = {
    'db_type': 'mysql',
    'username': '',
    'password': '',
    'host': '',
    'db_name': '',
    'port': 3306
}

REPORT_DB = {
    'db_type': 'mysql',
    'username': '',
    'password': '',
    'host': '',
    'db_name': '',
    'port': 3306
}
```  


#### 资源管理

##### 1. 数据资源

**在rf_data表新建记录 其中 `name` 作为唯一标识**

```Python
from rf_matrix.resource_manager import ResourceManager

table_name=rm.get_rm_data('每日销售') # 获取表名
rm.update_rm_data('每日销售') # 更新数据运算时间
```
##### 2. job管理

**在 rf_job表新建记录, 其中group, cat, job_name 作为分层辅助程序不敏感**
- work_dir: 入口模块所在目录
- module: 执行模块
- func: 入口函数
- hour: 执行时间批次
- priority: 同一批次下执行顺序
- job_status: 1启用, 0停用
- server_name: 运算服务器
- start_time: 最近一次运行开始时间, 自动更新	
- finish_time: 最近一次运行结束时间, 自动更新
- message: 为null运行成功, 报错显示错误信息, 自动更新