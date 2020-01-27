*A talk about*
### **WebApi** with **ASP.NET Core 3.1**
#### in **VS Code**
Jan Hanckel<br/><br/>
---

### What is new to ASP.NET Core?
* cross-platform
<!-- .element: class="fragment" -->
* open-source
<!-- .element: class="fragment" -->
----

### Supported hosting eviroments
* Kestrel
* IIS
* HTTP.sys
* Nginx
* Apache
* Docker
---

## WebApi Tutorial
Manage ToDo list
----

### Design of the App
![DesignApp](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-web-api/_static/architecture.png?view=aspnetcore-3.1)
----

### CLI
* Create the app
* Add packages for db connection
* open VS Code
```
  dotnet new webapi -o TodoApi
  cd TodoApi
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  dotnet add package Microsoft.EntityFrameworkCore.InMemory
  code -r ../TodoApi
```
----

* Generated folders<br/>
![Folders](/assets/folder_structure.png)
----

<!-- .slide: class="two-floating-elements" -->

* **.vscode** settings for VS Code
* **bin** and **obj** handled by .NET
* **Controllers** for the Api Controllers
* **Properties** settings for App launch

![Folders](/assets/folder_structure.png)
----

* add Models folder
* add TodoItem Model
```C#
namespace TodoApi.Models
{
    public class TodoItem
    {
        public long Id { get; set; }
        public string Name { get; set; }
        public bool IsComplete { get; set; }
    }
}
```
----

* add Context to store items
```C#
using Microsoft.EntityFrameworkCore;
namespace TodoApi.Models
{
    public class TodoContext : DbContext
    {
        public TodoContext(DbContextOptions<TodoContext> options)
            : base(options)
        {
        }

        public DbSet<TodoItem> TodoItems { get; set; }
    }
}
```
----

* register context in **Startup.cs**

```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddControllers();
}
```
* with added usings
```C#
using Microsoft.EntityFrameworkCore;
using TodoApi.Models;
```
----

* add packages for code generator
* install code generator
* create a controller for **TodoItem**

```
dotnet add package /
  Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller /
  -name TodoItemsController /
  -async -api -m TodoItem /
  -dc TodoContext -outDir Controllers
```
* new Controller with all CRUD functions
---

## DataAttributes
Route,<br/>
Method,...<br/>
are configured via DataAttributes
----

```C#
[Route("api/[controller]")]
[ApiController]
public class TodoItemsController : ControllerBase
{
  ...
}
```
* Route for this controller: api/todoitems
* **[controller]** = classname without controller suffix
----
```C#
// GET: api/TodoItems
[HttpGet]
public async Task<ActionResult<IEnumerable<TodoItem>>> GetTodoItems()
{
    return await _context.TodoItems.ToListAsync();
}

// GET: api/TodoItems/5
[HttpGet("{id}")]
public async Task<ActionResult<TodoItem>> GetTodoItem(long id)
{
  ...
}
```
----

* other methods
```C#
// POST: api/TodoItems
[HttpPost]
// DELETE: api/TodoItems/5
[HttpDelete("{id}")]
```

---

## Sources

* [Microsoft Documentation](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-3.1)
* [ASP.NET Core Repo](https://github.com/dotnet/aspnetcore)
* [Tutorial](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-3.1&tabs=visual-studio-code)
