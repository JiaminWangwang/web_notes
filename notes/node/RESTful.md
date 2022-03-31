> [RESTful文档地址](http://restful.p2hp.com/)
## **RESTful**
REST, 表现层状态转换。实际就是一种接口的命名规范。
url（请求路径）用来定义资源名称，type（请求类型）用来定义资源操作。
请求类型：
+ get: 获取、查询
+ post: 新增
+ put: 修改
+ delete: 删除

如果传递的数据中有一个唯一值（_id）, 我们会将_id放到url中。
```bash
get /students
get /students/001

delete /students/001
```
