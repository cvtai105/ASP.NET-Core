- dbContext được tạo và dispose mỗi khi hoàn thành một tác vụ database
- khi dbcontext dispose, ef sẽ reset context state và đưa trở lại pool

- Entities Convention:
    - Thuộc tính Id hoặc <classname>Id là PK
    - navigation property là thuộc tính đại diện cho entities khác có liên quan (bằng FK)
    - <otherclassname>Id hoặc <navigationpropertyname><navigationclassPK> là FK
    - nếu không định FK mà có navigation thì EF tự động tạo FK
    - many to many sẽ được ef tự động tạo bảng trung gian 
    - Các bảng được tự động map với entities. <tên bảng> = <tên entity>s

- [DatabaseGenerated(DatabaseGeneratedOption.None)] : tắt tự động tạo giá trị (ví dụ cho PK)

- services.AddDatabaseDeveloperPageExceptionFilter();

- add migration: dotnet ef migration add <tenmigration> 
- update database: dotnet ef database update
- reset database: dotnet ef database update 0  -> dotnet ef database update
- drop: dotnet ef database drop


[StringLength]
[DataType(DataType.Type)]
[DisplayFormat(DataFormatString="{0:yyyy-MM-dd}",ApplyFormatInEditMode=true)]
[Column("")]
[Required]
[Key]
[Range]
[PrimaryKey(nameof(),nameof())]

- mặc định, ef tạo cascade delete cho các non-nullable FK và many-to-many relationship
- handle concurren problems: https://learn.microsoft.com/en-us/aspnet/core/data/ef-mvc/concurrency?view=aspnetcore-8.0  

- FluentAPI:
    - FromSql
    - ExecuteSqlCommand (cần tránh sql injection)
    - AsNoTracking

## Advance: https://learn.microsoft.com/en-us/aspnet/core/data/ef-mvc/advanced?view=aspnetcore-8.0

- Có thể sử dụng ADO.NET với connection của EF để lấy dữ liệu không phải entities. _context.Database.GetDbConnection() hoặc tạo model mới và định nghĩa trong dbcontext.
     
- Repository pattern:
    - tách biệt dataAccess với business logic
    - thuận lợi cho việc kiểm thử
    - EF context class đã là thứ tách biệt .NET-code với database-code
    - EF context class hoạt động như một unit-of-work, nếu sử dụng repository cần phải thêm unit-of-work pattern cho các transactions
    - EF có tính năng implement TDD mà không cần repository (see in-memory datatabase)


- tracking:
    - giá trị nguyên bản được lưu khi được queried hoặc attached
    - DbContext.SaveChanges
    - DbContext.Entry
    - ChangeTracker.Entries
    - tăng performance: _context.ChangeTracker.AutoDetectChangesEnabled = false;

- Context không hỗ trợ thread-safe. Context ngốn ram nếu sử dụng lâu dài.
- Context đóng mở connection mỗi một query, có thể gây ra vấn đề hiệu năng, xem xét sử dụng Connection property.
- Trong Web application, hãy sử dụng một DBContext instance per request

- dotnet add package Microsoft.EntityFrameworkCore.SqlServer
- Khi EF core truy xuất entities, nó tải tất cả properties của entity đó, còn các navigation properties sẽ được set thành null

- Eager Loading: Load relation properties cùng với entities (sử dụng Include)
- Cần tránh hai loại loading sau trong web application vì nó sử dụng nhiều round trip tới db hơn.
    - Lazy Loading: Load entities. Và khi ứng dụng cần thì load thêm các relational propeties
    - Explicit Loading: khá giống lazy loading nhưng lazyload sẽ không load tự động như lazy loading, nó cần gọi Load method.
