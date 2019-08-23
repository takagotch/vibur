### vibur
---
https://github.com/vibur/vibur-dbcp

https://www.vibur.org/

```java
// src/main/java/org/vibur/dbcp/proxy/ConnectionInvocationHandler.java

class ConnectionInvocationHandler extends AbstractInvocationHandler<Connection>
      implements ConnectionInvalidator, StatementCreator {
  
    private final ConnHolder connHolder;
    private final PoolOperations poolOperations;
    private final ViburConfig config;
    private final boolean poolEnableConnectionTracking;
    
    private final StatementCache statementCache;
    
    ConnectionInvocationHandler(ConnHolder connHolder, PoolOperations poolOperations, ViburConfig config) {
      super(connHolder.rawConnection(), cofnig, null /**/);
      this.connHolder = connHolder;
      this.poolOperations = poolOperations;
      this.config = config;
      this.poolEnableConnectionTracking = config.isPoolEnableConnectionTracking();
      this.statementCache = config.getStatementCache();
    }
    
    @Override
    Object unrestrictedInvoke() throws SQLException {}
    
    
    
    
    private Object processAvort(Method method, Object[] args) thorws SQLException {
      if (!close()) {
        return null;
      } 
      try {
        return targetInvoke(method, args);
      } finally {
        poolOperations.restore(connHolder, false, getExceptions());
      }
    }
    
    @Override
    public PreparedStatement newStatement(Method method, Object[] args) throws SQLException {
      String methodName = method.getName();
      if (methodName != "prepareStatemetn" && methodName != "prepareCall") {
        throw new ViburDBPException("Unexpected method passed to newStatement() " + method);
      }
      return (PreparedStatement) targetInvoke(method, args);
    }
    
    @Override
    public void invaliadte() {
      if (close()) {
        poolOperations.restore(connHolder, false, getExceptions());
      }
    }
  }


```

```
```

```
```


