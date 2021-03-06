---
title: "ããåç¨¿"
emoji: "ð¤®"
type: "tech" # tech: æè¡è¨äº / idea: ã¢ã¤ãã¢
topics: ['csharp', 'asukacs']
published: false
---

# è¨ãè¨³

ããã§ã¯ãããè¨äºã§æ²¡ã«ãããã®ã®å¥ã®è¨äºã§ä½¿ããããããªãã³ã¼ããç½®ãã¨ãã¾ãã

# åé¡

ãã­ã°ã©ã ä¸ã®ã¡ã¢ãªãã­ã¼ã«ã«ã³ã³ãã¥ã¼ã¿ä¸ã®ãã¡ã¤ã«ããã¼ã¿ãã¼ã¹ï¼MySQLï¼ãã¯ã©ã¦ãã®ï¼ç¨®é¡ã®ã¹ãã¬ã¼ã¸ã®ãã¼ã¿ãèª­ã¿æ¸ãããã¯ã©ã¹ãä½æãã¾ãããããã®ã¯ã©ã¹ã«ã¯ãå±éãã¦ä»¥ä¸ã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ãå®è£ãããã¨èãã¦ãã¾ãããªããã­ã°ã©ã ä¸ã®ã¡ã¢ãªã¨ã¯ãåã«C#ã®ææ³ä¸ã®å¤æ°ãããã¾ãã
ããããã®ã¾ã¾ä½¿ç¨ãã¦ãåé¡ã¯ãªãã§ããããï¼

```cs
interface IRepository : IAsyncDisposable
{
  Task<bool> IsMaintenancingAsync();

  Task AuthenticateAsync(Certificate certificate);

  Task ConnectAsync();

  Task StartTransactionAsync();

  Task<Dictionary<string, string>> ReadAsync(string key);

  Task WriteAsync(string key, Dictionary<string, string> data);

  Task CommitTransactionAsync();
}
```

```mermaid
classDiagram
  IAsyncDisposable <|.. IRepository
  Certificate -- IRepository
  class IAsyncDisposable {
    +DisposeAsync()
  }
  class IRepository {
    +IsMaintenancingAsync() bool
    +AuthenticateAsync(Certificate)
    +ConnectAsync()
    +StartTransactionAsync()
    +ReadAsync(string) Dictionary~string_string~
    +WriteAsync(string, Dictionary~string_string~)
    +CommitTransactionAsync()
  }
```

## ã©ããåé¡ï¼

å®ã¯ãã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ã«ãã£ã¦æ½è±¡åããããªãã¸ã§ã¯ãã¯ããã®ã¾ã¾å¼ã³åºãã¦ãåé¡ã¯ããã¾ããããªããªãããããã®å·è±¡ã¯å±éã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ï¼æ½è±¡ï¼ãå®è£ãã¦ããããã§ãã

```cs
await using var repo = Connection.Default.CreateNew();

if (await repo.IsMaintenancingAsync())
{
  return;
}

var cert = Certificate.Default;

try
{
  await repo.AuthenticateAsync(cert);
  await repo.ConnectAsync();
  await repo.StartTransactionAsync();

  var data = await repo.ReadAsync("Human");
  data["Name"] = "è¥¿æãã";
  await repo.WriteAsync(data);

  await repo.CommitTransactionAsync();
}
catch
{
  // ...
}
```

ããã§ã¯ããã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ãå®è£ããã¯ã©ã¹ã¯ã©ãã§ããããã
ãã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ã®å·è±¡ã¯ï¼ç¨®é¡ããã¾ãã

* ãã­ã°ã©ã ä¸ã®ã¡ã¢ãªï¼å¤æ°ï¼
* ã­ã¼ã«ã«ã³ã³ãã¥ã¼ã¿ä¸ã®ãã¡ã¤ã«
* ãã¼ã¿ãã¼ã¹ï¼MySQLï¼
* ã¯ã©ã¦ã

ãããã¯å¿ããããä¸è¨ã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ã®å¨ã¦ã®ã¡ã½ãããå®è£ãã¾ãããå®è£ã®éã«ã¯ãä»¥ä¸ã®å·®ç°ãçºçãã¾ãã

* ã¡ã¢ãªãã­ã¼ã«ã«ãã¡ã¤ã«ã§ã¯ãµã¼ãã¼æ¥ç¶ã¨ããéç¨ãçºçããªããããèªè¨¼ã¨ããæ¦å¿µãçºçãã¾ãã
* ãã©ã³ã¶ã¯ã·ã§ã³ã¯ãã¼ã¿ãã¼ã¹ã«ã®ã¿å¯è½ã§ãããä»ã®ï¼ã¤ã§ã¯ã§ãã¾ãã
* æ¸ãè¾¼ã¿ã¨ã³ããããåããã®ã¯ãã¼ã¿ãã¼ã¹ã®ç¹æ§ã§ããããã¼ã¿ãã¼ã¹ã§ã¯`CommitTransactionAsync`ãä»ã®ï¼ã¤ã§ã¯`WriteAsync`ãå®è¡ããæç¹ã§æ¸ãè¾¼ã¾ãã¾ã

ç¹ã«ä¸çªæå¾ã®å·®ç°ã¯éå¤§ã§ãã`WriteAsync`ã¨`CommitTransactionAsync`ã®ã©ã¡ãã«ãã£ã¦å®éã®ä¿å­ããªãããããã**å®è£ã«ãã£ã¦ç°ãªã**ããã§ããã©ã¡ããä¸æ¹ã«æãããã¨ã¯ä¸å¯è½ã§ãããªããªããã¼ã¿ãã¼ã¹ã«ããã¦ããã©ã³ã¶ã¯ã·ã§ã³ã¯ãªãã·ã§ã³ã§ããããããå®è¡ããªãã¦ãä¿å­å¯è½ã ããã§ããããã¯ãå®è£ãå®å¨ã«ç½®æå¯è½ã§ãªããã¨ãæå³ãã¾ãã

ä»ã®ï¼ã¤ã®å·®ç°ã¯ãããããã®å®è£ã«ä¸è¦ãªã¡ã½ãããå­å¨ãããã¨ãç¤ºãã¾ããéå¸¸ããããã¯ã¡ã½ããã®ä¸­ãç©ºã«ãã¾ãã

```cs
class InMemoryRepository : IRepository
{
  public async Task<bool> IRepository.IsMaintenancingAsync() => false;

  public async Task IRepository.AuthenticateAsync(Certificate certificate) { }

  public async Task IRepository.ConnectAsync() { }

  public async Task IRepository.StartTransactionAsync() { }

  // ...
}
```

ããã¯ãå·è±¡ã¡ã½ãããå®è£ãããã­ã°ã©ããä¸å®ã«ããã¾ããæ°ããå·è±¡ãä½æããæãã©ã®ã¡ã½ãããå¿è¦ã§ã©ã®ã¡ã½ãããä¸è¦ãªã®ãææ¡ããä¸ã§å®è£ãè¡ããªããã°ããã¾ãããä»åã®ä¾ã§ã¯ç¤ºããã¦ãã¾ããããä¾ãã°å¼æ°ã¨ãã¦æ¸¡ããããªãã¸ã§ã¯ãã®ç¶æãå¤æ´ããå¿è¦ãããæãªã©ã¯ãç¶æ³ãããã«è¤éã«ããã¾ããã¤ã³ã¿ã¼ãã§ã¼ã¹ãè¤éã§ããã°ããã»ã©ãå®è£å´ã®å­¦ç¿ã³ã¹ãã¯å¢å¤§ããã¡ã³ããã³ã¹ãç©éã«ãã¾ãã

ã¾ããããã¯è«ççã§ã¯ããã¾ããããå¿è¦ä»¥ä¸ã«å¤§ããªã¤ã³ã¿ã¼ãã§ã¼ã¹ã¯ãã­ã°ã©ãã®å¿çã«ãä½ç¨ãããããå®è£ãèºèºããã¾ããæã«ã¯ãä½æ¥­æéãéå°ã«è¦ç©ãããã¾ããããã¯å¹ççã¨ã¯ããã¾ããã
ãã ããä»åã®ä¾ã¯ã¡ã½ããã®æ°ãå°ãªãã®ã§åã«å¿ççãªåé¡ã§æ¸ã¿ã¾ãããæ°ãããã«å¢ããã¨ãããã¯æè¡çè² åµã¸ã¨å¤åãã¾ããã»ã¨ãã©ã®å·è±¡ã«ããã¦ä¸è¦ãªã¡ã½ãããå¢ãããã¨ã¯ãããããã®å·è±¡ã®å®è£ã³ã¹ããå¢ããã ãã§ãªããæ½è±¡ã®å¼ã³åºãå´ã«ãå¿è¦ä»¥ä¸ã®æ½è±¡ã«å¯¾ããç¥è­ãè¦æ±ããã³ã¼ããåé·ã«ãªãã¾ãã

## ã¢ãã­ã¼ã

C#ã§ã¯ã`using`ã¾ãã¯`await using`ã«ãã£ã¦å®£è¨ãããã¤ã³ã¹ã¿ã³ã¹ã¯ãä¾å¤çºçæãå«ããã¹ã³ã¼ãçµäºæã«ãå¿ã`Dispose`ã¾ãã¯`DisposeAsync`ã¡ã½ãããå¼ã³åºããã¦ç ´æ£ããã¾ãããã®ãã¨ãçããã¦è¨­è¨ãè¡ãã¾ãã

```cs
interface IRepository
{
  Task<bool> IsMaintenancingAsync();

  Task<IRepositoryConnection> ConnectAsync(Certificate certificate);
}

interface IRepositoryConnection : IAsyncDisposable
{
  Task<IRepositoryQuery> StartQueryAsync();

  Task<IRepositoryQuery> StartQueryWithTransactionAsync();
}

interface IRepositoryQuery : IAsyncDisposable
{
  bool IsTransaction { get; }

  Task<Dictionary<string, string>> ReadAsync(string key);

  Task WriteAsync(string key, Dictionary<string, string> data);
}
```

```mermaid
classDiagram
  IAsyncDisposable <|.. IRepositoryConnection
  IAsyncDisposable <|.. IRepositoryQuery
  Certificate -- IRepository
  IRepository -- IRepositoryConnection
  IRepositoryConnection -- IRepositoryQuery
  class IAsyncDisposable {
    +DisposeAsync()
  }
  class IRepository {
    +IsMaintenancingAsync() bool
    +ConnectAsync(Certificate) IRepositoryConnection
  }
  class IRepositoryConnection {
    +StartQueryAsync() IRepositoryQuery
    +StartQueryWithTransactionAsync() IRepositoryQuery
  }
  class IRepositoryQuery {
    +bool IsTransaction
    +ReadAsync(string) Dictionary~string_string~
    +WriteAsync(string, Dictionary~string_string~)
  }
```

ããã¯ãã¡ã¢ãªããã¡ã¤ã«ãªã©èªè¨¼ãè¦æ±ããªãæ¥ç¶ã«ããã¦ã¯é½åã®ããæ½è±¡ã§ãããªããªãããããã®æ¥ç¶ããèªè¨¼ãå¿è¦ã¨ããªãæ¥ç¶ãã¨ãã¦å®è£ã§ããã¾ãããããå®¹æã«ä½¿ãåããããã§ããããã¯ãä¸é¨ã®å®è£ãç°¡ç¥åããã¤ã³ã¿ã¼ãã§ã¼ã¹ãå®è£ãããã­ã°ã©ãã®è² æãè»½æ¸ãã¾ãã
ãã¨ãã¨ã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ããã®ã¾ã¾ä½¿ãã®ã§ããã°ãä»¥ä¸ã®å®è£ã¯ããããã®ã¯ã©ã¹ãææ¸ãã§è¨è¿°ããããã¾ãã¯ã³ã³ãã¸ã·ã§ã³ãä½¿ããªããã°ããã¾ããã§ããã

```cs
class ConnectionWithoutAuthentication : IRepositoryConnection
{
  private readonly Func<IRepositoryQuery> queryGenerator;

  public ConnectionWithoutAuthentication(Func<IRepositoryQuery> queryGenerator)
  {
    this.queryGenerator = queryGenerator;
  }

  public async Task<IRepositoryQuery> StartQueryAsync() => this.queryGenerator();

  public async Task<IRepositoryQuery> StartQueryWithTransactionAsync() => await this.StartQueryAsync();

  // ãã£ã±ãç©ºã¡ã½ãããçºçããã®ã¯ãæå¬
  public async Task DisposeAsync() { }
}
```

ã¾ãããã©ã³ã¶ã¯ã·ã§ã³ã«ãã£ã¦`WriteAsync`ã¨`CommitTransactionAsync`ã®æåãå¤ããåé¡ã«ã¤ãã¦ã¯ãæ ¹æ¬çãªè§£æ±ºã§ã¯ããã¾ãããã`IRepositoryQuery`ã¤ã³ã¿ã¼ãã§ã¼ã¹ã«`IsTransaction`ãã­ããã£ãæ°è¨­ãã¾ããã
ãã¨ãã¨ã®ã¤ã³ã¿ã¼ãã§ã¼ã¹ã¯ãã©ã³ã¶ã¯ã·ã§ã³ãæç¶ãçã«éå§ããããããã­ã°ã©ããåå¾ã®å¦çãææ¡ããå¿è¦ãããã¾ãããä»åã®ã¢ãã­ã¼ãã«ããã¦ãæ¥ç¶ãã®ãã®ãæ½è±¡åããã¦æ°ãããªãã¸ã§ã¯ãã«åãåºããã¾ããããã©ã³ã¶ã¯ã·ã§ã³ã®ä½¿ç¨ç¶æ³ã¯ãæ¥ç¶ã®ç¶æã¨ãã¦ãããã¦æä¾ãããæ½è±¡ã®å¼ã³åºãå´ã¯ãããä»»æã®ã¿ã¤ãã³ã°ã§ç¢ºèªã§ãã¾ãããã®ããããã®æ¥ç¶ãå¥ã®ã¡ã½ããã«æ¸¡ãã¦ããåé¡ãªãå®è¡ãããã³ã¼ããæ¸ããã¨ãå®¹æã«ãªãã¾ãã
ãã¼ã¿ãã¼ã¹ã§ã¯`StartQueryAsync`ã¨`StartQueryWithTransactionAsync`ã§ç°ãªãã¯ã©ã¹ã®ã¤ã³ã¹ã¿ã³ã¹ãããä»¥å¤ã§ã¯åä¸ã¯ã©ã¹ã®ã¤ã³ã¹ã¿ã³ã¹ãè¿å´ããã®ã¯è¨ãã¾ã§ãããã¾ããã