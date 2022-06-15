实验三：Persistence Ignorance
-----------------------------

-  滕佳倩 201931990407
-  吴支飞 201931990232
-  温启涛 201931990228
-  杨佳宁 201931990414
-  王景瑶 201931990508

实验目的
~~~~~~~~

-  理解仓库模式
-  理解服务层
-  理解将业务逻辑从数据库存储技术中分离出来的重要性

材料和方法
~~~~~~~~~~

1. 团队分工

   -  滕佳倩：编写实验报告
   -  吴支飞：回答实验问题
   -  温启涛：录制演示视频
   -  杨佳宁：编写完整代码
   -  王景瑶：编写实验报告

2. 开发语言及工具

   -  Windows 10操作系统，Python语言开发环境
   -  pytest测试工具

结果
~~~~

-  repository.py

.. code:: python

   # repository.py
   import abc

   import model
   import os,pickle



   class AbstractRepository(abc.ABC):
       @abc.abstractmethod
       def add(self, batch: model.Batch):
           raise NotImplementedError

       @abc.abstractmethod
       def get(self, reference) -> model.Batch:
           raise NotImplementedError


   class SqlAlchemyRepository(AbstractRepository):
       def __init__(self, session):
           self.session = session

       def add(self, batch):
           self.session.add(batch)

       def get(self, reference):
           return self.session.query(model.Batch).filter_by(reference=reference).one()

       def list(self):
           return self.session.query(model.Batch).all()

   class PickleRepository(AbstractRepository):
       def __init__(self,path=None):
           self.path=path

       def add(self,batch):
           _batchs=self.list()
           _batchs.append(batch)
           with open(self.path,'wb') as f:
               pickle.dump(_batchs,f)

       def get(self,reference):
           pass

       def list(self):
           with open(self.path,'rb') as f:
               return list(pickle.load(f))

-  测试结果

   .. figure:: https://s2.loli.net/2022/06/15/JPReO1hGiSgap3M.png
      :alt: GDN_9OBO_L_WFZDXN6__FWF.png

      GDN_9OBO_L_WFZDXN6__FWF.png

-  `视频链接 <https://cloud.zjnu.edu.cn/share/1b83cb08ddc3dc32ab7018b1bc>`__

讨论分析
~~~~~~~~

1. What is the difference between the textbook test services.py and my
   test services.py?

   -  课本中的代码并未使用仓库模式，将测试和原始SQL混合在一起。另一份代码采用仓库模式，可以快速进行单元测试

2. Has the service layer been affected after we have chosen to use
   another implementation for the Repository Pattern? Can we say that
   the service layer is ignorant of the persistence?

   -  服务层进行了拆分，分为应用程序服务(服务层)和领域服务。应用程序服务(服务层)。它的职责是处理来自外部的请求并安排操作，一般有

      -  从数据库中获取一些数据
      -  更新领域模型
      -  持久化更改

   -  领域服务，它主要职责是操作实体与值对象之间的逻辑
   -  服务层并没有忽略持久化，应用程序服务仍在安排持久化更改

3. What is the benefit of separating business logic from infrastructure
   concerns? Where is the business logic defined, and where is the
   infrastructure defined? Tell me the Python file name(s).

   -  好处：

      -  使用服务层可以快速的编写测试，也可以非常大胆的重构我们的领域模型。就可以尝试新的设计，而不会因为这样重写大量的测试。
      -  便于进行争对服务的单元测试，只有极少数端到端测试和集成测试

   -  business logic defined：services.py
   -  infrastructure defined：repository.py

参考文献
~~~~~~~~

[1]Harry Percival and Bob Gregory. Architecture Patterns with Python.
