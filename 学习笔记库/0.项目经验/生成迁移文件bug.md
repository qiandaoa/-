![ 输出 aaa-2021-9-1718:04:33.png](https://gitee.com/lianzengqian/picture/raw/master/%20%E6%A0%BC%E5%BC%8F%201721562077150-2024-7-2119:41:17.png%20/%20%E8%BE%93%E5%87%BA%20aaa-2021-9-1718:04:33.png)

解决方案:

```
using Microsoft.EntityFrameworkCore.Design;
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Blog.Core.EFCore
{
    public class DbContextDesignTimeFactory : IDesignTimeDbContextFactory<BlogDbContext>
    {
        public BlogDbContext CreateDbContext(string[] args)
        {
            DbContextOptionsBuilder<BlogDbContext> builder = new DbContextOptionsBuilder<BlogDbContext>();
            builder.UseNpgsql("host=101.133.150.189;port=5432;database=blog;uid=blog;pwd=2071260354");
            return new BlogDbContext(builder.Options);
              
        }
    }
}

```

[EFcore迁移报错：“Unable to create an object of type ‘MyDbContext‘. For the different patterns supported ”_unable to create an object of type 'mydbcontext'. -CSDN博客](https://blog.csdn.net/dawfwafaew/article/details/123939182)

由于默认的 Main 函数被修改了，执行 Add-Migration 命令，会发生错误。