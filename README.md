# Second Level Cache for Entity Framework 6.1

Entity Framework does not currently support caching of query results. A sample EF Caching provider is available for Entity Framework version 5 and earlier but due to changes to the provider model this sample provider does not work with Entity Framework 6 and newer. This project is filling the gap by enabling caching of query results for Entity Framework 6.1 applications. 

# How to get it

You can get it from NuGet - just install the [EntityFramework.Cache NuGet package](http://www.nuget.org/packages/EntityFramework.Cache)

# How to use it

The project uses a combination of a wrapping provider and a transaction interceptor. A simple [InMemoryCache](https://github.com/moozzyk/EFCache/blob/master/EFCache/InMemoryCache.cs) is included in the project. To use it you need first configure EF using code based configuration. Here is an example of how such a configuration looks like.

```C#
public class Configuration : DbConfiguration
{
  public Configuration()
  {
    var transactionHandler = new CacheTransactionHandler(new InMemoryCache());

    AddInterceptor(transactionHandler);

    var cachingPolicy = new CachingPolicy();

    Loaded +=
      (sender, args) => args.ReplaceService<DbProviderServices>(
        (s, _) => new CachingProviderServices(s, transactionHandler, 
          cachingPolicy));
  }
}
```

You can find more details in my [blogpost](http://blog.3d-logic.com/2014/03/20/second-level-cache-for-ef-6-1/)
