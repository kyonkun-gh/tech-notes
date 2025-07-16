# 設計模式(Design Pattern)

- **單例模式(Singleton)**

要有靜態(static)類別與屬性

建構子必須要 private，避免被 new出新物件

```Java
class Singleton {
    private static Singleton instance;

    private Singleton(){}

    public static Singleton getInstance() {
        if ( instance == null ) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

