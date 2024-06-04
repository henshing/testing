# 自动化框架v2.0仓库位置
https://github.com/kern-crates/testing

# 整体流程描述
测试流水线步骤可简单描述为：当github代码仓库发生push操作时，会触发jenkins流水线中的触发器，进而流水线会根据码仓中的jenkinsfile文件执行流水线操作。流水线操作总结如下：
1、环境变量设置（包含用户需要自定义的各项参数，包括个人github码仓、待测主仓名、接收报告邮件地址等）
2、码仓代码检出
3、码仓编译测试（包括自动化测试框架嵌入及测试用例执行）
4、测试报告生成并合并
5、测试报告邮件发送

# 自动化测试框架pytest及测试报告生成应用allure作用
pytest文件即为实现测试自动化的框架。在该文件目录中，

















# v1.0仓库位置
https://github.com/buhenxihuan/Starry


# 整体流程描述
github action目前在仓库发生push和pr时会自动触发，同时每个四个小时会定时触发，aciton触发后，也可以手动触发，按照auto_test.yml中的流程，先检出整个仓库的代码到aciton的机器中，而后进行前期环境准备，完成后调用pytest和allure进行测试并收集报告，最后通过allure将报告发送到github pages中进行展示。

# auto_test.yml的流程见相应注释 
https://github.com/buhenxihuan/Starry/blob/x86_64/.github/workflows/auto_test.yml

# pytest和allure作用
pytest目录中 config.py文件为pytest执行时读取的配置文件，其中按照类别分别定义了clippy测试的配置，unikernel测试的配置，monolithic测试配置以及一些需要在报告中展示的信息

关键代码为pytest/testcase目录下的test_arceos.py中的代码，具体作用请参见pytest的使用说明（https://learning-pytest.readthedocs.io/zh/latest/index.html ），简单来说就是通过pytest封装了原先从build_img.sh到make执行的过程，全部交由pytest和allure进行接管，从而可以方便的进行结果的收集与过滤

# v1.0使用方法

1. 将auto_test.yml放入.github/workflows(不存在则新建该目录)目录中
2. 将pytest目录放入顶层目录下
3. 向顶层Makefile中加入以下两行
```shell
TC ?= busybox
export AX_TC=$(TC)
```
4. 确保apps目录下有微内核和宏内核测例的入口
5. 选择push、pr、定时或手动触发
6. 若要在线查看报告，请使用github pages进行搭建（具体方法参照 https://docs.github.com/zh/pages/getting-started-with-github-pages/creating-a-github-pages-site ），在auto_test.yml中定义了一个github pages的分支名为gh-pages_ci，若要更改，请查找该文件中的gh-pages_ci字段自行进行替换
7. 一个可以查看的示例 https://buhenxihuan.github.io/Starry/
