定义一个新的工作流
===================

本节介绍如何定义只有一个任务的工作流。

创建工作流文件
---------------

创建一个 Python 文件 **test.py**

.. code-block:: python
    :linenos:

    import sys
    from pathlib import Path

    from takler.core import Flow
    from takler.tasks.shell import ShellScriptTask
    from takler.visitor import pre_order_travel, PrintVisitor


    TAKLER_HOME = "/g11/wangdp/project/course/takler/tutorial"


    def create_flow():
        flow1 = Flow("flow1")
        flow1.add_parameter("TAKLER_HOME", TAKLER_HOME)
        task1 = flow1.add_task(ShellScriptTask("task1"))
        task1.add_parameter("TAKLER_SCRIPT", Path(TAKLER_HOME, "task1.takler"))
        return flow1


    if __name__ == "__main__":
        flow1 = create_flow()
        pre_order_travel(flow1, PrintVisitor(sys.stdout))


上述代码定义了一个叫做 `flow1` 的工作流，包含一个叫做 `task1` 的任务。

逐行解释上述代码：

- 1-2：导入 Python 自带包
- 4-6：导入 takler 包中对象
    - ``Flow``：工作流类
    - ``ShellScriptTask``：Shell 脚本任务类
    - ``pre_order_travel()`` 函数：前序遍历工作流
    - ``PrintVisitor`` 类：打印节点信息
- 9：``create_flow()`` 函数创建只有一个任务 (task1) 的工作流 (flow1)
- 13：定义工作流 ``flow1``
- 14：为 ``flow1`` 定义变量 ``TAKLER_HOME``，该变量定义工作流生成的作业文件的保存目录
- 15：定义 Shell 脚本任务 ``task1``
- 16：为 ``task1`` 定义变量 ``TAKLER_SCRIPT``，该变量定义 Shell 脚本任务对应的脚本目录，本例中为

.. code-block::

    /g11/wangdp/project/course/takler/tutorial/task1.takler

- 17：``create_flow()`` 函数返回工作流 ``flow1`` 对象
- 20：定义直接运行脚本会执行的代码
- 21：创建工作流
- 22：``pre_order_travel`` 与 ``PrintVisitor`` 结合会打印工作流的树形结构

运行上述脚本

.. code-block:: bash

    python test.py


会打印工作流结构

.. code-block::

    |- flow1 [unknown]
      |- task1 [unknown]


练习
-----

1. 创建工作流定义文件 **test.py**
2. 运行脚本