常用的FluentApi方法及其作用：

| 配置                         | Fluent API 方法                                              | 作用             |
| ---------------------------- | ------------------------------------------------------------ | ---------------- |
| 架构相关配置                 | HasDefaultSchema()                                           | 数据库的默认架构 |
| ComplexType()                | 把一个类配置为复杂类型                                       |                  |
| 实体相关配置                 | HasIndex()                                                   | 实体的的索引     |
| HasKey()                     | 实体的主键（可其实现复合主键，[Key]在EF core中不能实现复合主键） |                  |
| HasMany()                    | 1对多的或者 多对多关系                                       |                  |
| HasOptional()                | 一个可选的关系，这样配置会在数据库中生成一个可空的外键       |                  |
| HasRequired()                | 一个必有的关系，这样配置会在数据库中生成一个不能为空的外键   |                  |
| Ignore()                     | 实体或者实体的属性不映射到数据库                             |                  |
| Map()                        | 设置一些优先的配置                                           |                  |
| MapToStoredProcedures()      | 实体的CUD操作使用存储过程                                    |                  |
| ToTable()                    | 为实体设置表名                                               |                  |
| 属性相关配置                 | HasColumnAnnotation()                                        | 给属性设置注释   |
| IsRequired()                 | 在调用SaveChanges()方法时，属性不能为空                      |                  |
| IsOptional()                 | 可选的，在数据库生成可空的列                                 |                  |
| HasParameterName()           | 配置用于该属性的存储过程的参数名                             |                  |
| HasDatabaseGeneratedOption() | 配置数据库中对应列的值怎样生成的，如计算，自增等             |                  |
| HasColumnOrder()             | 配置数据库中对应列的排列顺序                                 |                  |
| HasColumnType()              | 配置数据库中对应列的数据类型                                 |                  |
| HasColumnName()              | 配置数据库中对应列的列名                                     |                  |
| IsConcurrencyToken()         | 配置数据库中对应列用于乐观并发检测                           |                  |

使用FluentApi对领域类做了以下配置：

```
public class SchoolContext : DbContext
    {
        public SchoolContext() : base()
        {
        }
        public DbSet<Student> Students { get; set; }
        public DbSet<Grade> Grades { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            //设置默认架构
            modelBuilder.HasDefaultSchema("Admin");
            //设置主键
            modelBuilder.Entity<Student>().HasKey<int>(s => s.StudentKey);
            
            //设置不映射的属性
            modelBuilder.Entity<Student>().Ignore(s => s.Height);
            
            //设置DateOfBirth
            modelBuilder.Entity<Student>().Property(p => p.DateOfBirth)
                .HasColumnName("birthday")    //列名为birthday
                .HasColumnType("datetime2")   //数据类型是datetime类型
                .HasColumnOrder(3)            //顺序编号是3
                .IsOptional();                //可以为null

            //设置姓名
            modelBuilder.Entity<Student>().Property(s => s.StudentName)
                .HasMaxLength(20)             //最长20
                .IsRequired()                 //不能为null
                .IsConcurrencyToken();        //用于乐观并发检测,delete或者update时，这个属性添加到where上判断是否并发              
        }
    }
```

