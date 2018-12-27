#Starlette  
##介绍  
Starlette是一个轻量级的ASGI框架/工具包，它是构建高性能的asyncio服务的一个理想选择。

它是生产就绪的，并且能给你带来以下这些：
  
- 高性能。  
- WebSocket支持。  
- GraphQL支持。  
- 进程内后台任务。  
- 启动和关闭事件。  
- 基于请求的测试客户端。 
- 对CORS，GZip，静态文件，流的响应。  
- 对Session和Cookie的支持。  
- 100％测试覆盖率。  
- 100％解释语言类型代码库。  
- 0硬性依赖  
 
##需要
Python 3.6+
##安装  
`$ pip3 install starlette`  
你还需要安装一个ASGI服务器，如：[uvicorn](https://www.uvicorn.org/), [daphne](https://github.com/django/daphne/), 或 [hypercorn](https://pgjones.gitlab.io/hypercorn/)
`$ pip3 install uvicorn`  
##例子  
    from starlette.applications import Starlette
    from starlette.responses import JSONResponse
    import uvicorn

    app = Starlette()

    @app.route('/')
    async def homepage(request):
    return JSONResponse({'hello': 'world'})

    if __name__ == '__main__':
    uvicorn.run(app, host='0.0.0.0', port=8000)
有关更完整的示例，请[参见此处](https://github.com/encode/starlette-example)。
##依赖
Starlette没有任何硬性依赖，但以下是可选的：  

* [requests](http://docs.python-requests.org/en/master/)-如果你想使用，则需要`TestClient`。  
* [aiofiles](https://github.com/Tinche/aiofiles)- 如果您想使用,则需要`FileResponse`或`StaticFiles`。  
* [jinja2](http://jinja.pocoo.org/) - 如果要使用默认模板配置则使用它。  
* [python-multipart](https://andrew-d.github.io/python-multipart/)- 如果要支持表单解析，则用到`request.form()`。  
* [itsdangerous](https://pythonhosted.org/itsdangerous/)- 需要`SessionMiddleware`支持。  
* [sqlalchemy](https://www.sqlalchemy.org/)- 需要`DatabaseMiddleware`支持。  
* [pyyaml](https://pyyaml.org/wiki/PyYAMLDocumentation)- 需要`SchemaGenerator`支持。  
* [graphene](https://graphene-python.org/)- 需要`GraphQLApp`支持。  
* [ujson](https://github.com/esnme/ultrajson)- 如果您想使用，则需要`UJSONResponse`。  
##框架或工具包  
Starlette旨在用作完整框架或ASGI工具包。您可以单独使用其任何组件。 
 
    from starlette.responses import PlainTextResponse

    class App:
    def __init__(self, scope):
        self.scope = scope

    async def __call__(self, receive, send):
        response = PlainTextResponse('Hello, world!')
        await response(receive, send)   
运行`App`应用程序`example.py`

    $ uvicorn example:App
    INFO: Started server process [11509]
    INFO: Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
##模块性
Starlette的模块性体现在促进了构建可在任何ASGI框架之间共享的可重用组件。这应该能够实现共享中间件和可安装应用程序的生态系统。

干净的API分离也意味着更容易理解每​​个组件。
##性能
独立的TechEmpower基准测试显示，在Uvicorn下运行的Starlette应用程序是[最快的Python框架之一](https://www.techempower.com/benchmarks/#section=data-r17&hw=ph&test=fortune&l=zijzen-1)。（*）  

对于高吞吐量负载，您应该：  

* 确保安装`ujson`和使用`UJSONResponse`。
* （使用uvicorn工人类使用Gunicorn运行。）
* 每个CPU核心使用一个或两个工作者。（您可能需要对此进行试验。）
* 禁止访问日志。  
例如：  
`gunicorn -w 4 -k uvicorn.workers.UvicornWorker --log-level warning example:app`

一些ASGI服务器也提供纯Python实现，如果您的应用程序代码具有CPU约束的部分，你也可以将其在`PyPy`下运行。 
 
以编程方式：  
`uvicorn.run(..., http='h11', loop='asyncio')`  

或使用Gunicorn：
`gunicorn -k uvicorn.workers.UvicornH11Worker ...`


  


