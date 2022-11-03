### k8s部署默认helm快速部署模板  
    使用helm方便部署时动态处理k8s.yaml中的配置，可自由的通过-f xxx.yaml文件 --set VAR1=xxx 等方式叠加覆盖
#### demo 示例 参考ocean项目
>项目使用本helm模板部署时参考values-demo.yaml设置自定义变量.完整变量参考values.yaml

>将values-demo.yaml拷贝到项目的根目录下，改名为helm-values.yaml    

>项目 build时额外将helm-values.yaml文件进行归档.
    

#### helm替换kubectl部署示例
第一次部署时需要将执行install，后续都是upgrade   
```bash
#超长的环境变量,太多字符导致无法使用--set替换，需要放到文件中
start_other_opts="xxxxxxxx"

#将变量写入文件
echo $start_other_opts > ./start_other_opts.txt


helm upgrade appNmae ./ \ #执行升级，./为模板目录，也可以是模板tgz文件。新的k8s中安装时将upgrade替换为install
--namespace xxxspace \ #指定namespace
-f ./helm-values.yaml \ #将项目中的自定义变量覆盖模板
--set image.repository=xxxxname \ #指定服务镜像
--set image.tag=xxxversion \ #指定版本号
--set env[0].name=OPTS_ENV_MARK \ #设置容器内使用的环境变量名
--set env[0].value=dev \ #设置OPTS_ENV_MARK的值为test
--set env[1].name=OPTS_OTHER \ #设置容器内使用的环境变量名
--set-file env[1].value=./start_other_opts.txt #设置OPTS_OTHER的值为start_other_opts.txt文件的内容
```