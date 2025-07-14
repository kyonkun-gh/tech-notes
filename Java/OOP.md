# 物件導向程式設計(Object-Oriented Programming)

- **建構式**： 建立物件(new)時用來初始化物件狀態
- **預設建構式**： 當沒有明確寫出任何建構式時，編譯器預設會提供一個無參數、空內容的建構式，稱為預設建構式

- **物件導向三大特性**： 封裝、繼承、多型

- **封裝 (Encapsulation)**： 封裝是將物件的資料（變數）和行為（方法）包裝在一起，並且隱藏內部細節，只對外暴露必要的接口。
使用者不需要了解物件的內部實作細節，只需使用公開的方法來操作物件
```Java
public class Person {
    private String name;    // 私有變數，封裝內部狀態

    public String getName() {   // 公開方法，提供存取接口
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

- **多載 (Overloading)**： 在同一類別中，方法名稱相同，但參數型別、數量不同，稱為多載
```Java
class Hello {
    public void print() {
	    System.out.println( "" );
    }
    
    public void print(String hello) { // 與 print方法名稱相同，但額外提供 String hello參數
	    System.out.println( hello );
    }
}
```

- **繼承 (Inheritance)**： 繼承是子類別繼承父類別的屬性與方法，重用父類別程式碼，並能擴充或覆寫（Override）父類別功能
```Java
class Animal {
}
class Dog extends Animal {  // Dog使用 extends關鍵字繼承父類別 (Animal)
}
```

- **覆寫 (Overriding)**： 子類別繼承父類別後，可以重新定義父類別的方法，稱為覆寫
```Java
class Animal {
	public void move() {    // 父類別提供 move方法
		System.out.println("move");
	}
}
class Dog extends Animal {
    @Override               // 不是必要，有寫的話，編譯器會檢查
	public void move() {    // 子類別重新覆寫父類別 move方法
		System.out.println("run");
	}
}
```

- **多型 (Polymorphism)**： 父類別變數可以指向子類別的物件，執行時根據物件型別呼叫對應方法，使得相同程式碼有不同行為，稱為多型
```Java
class Animal {
	public void move() {
		System.out.println("move");
	}
}
class Dog extends Animal {
	@Override
    public void move() {
		System.out.println("run");
	}
}
class Bird extends Animal {
    @Override
	public void move() {
		System.out.println("fly");
	}
}

Animal dog = new Dog();     // Animal變數，實際是 Dog物件
Animal bird = new Bird();   // Animal變數，實際是 Bird物件
dog.move();                 // 執行 Dog move方法，輸出 run
bird.move();                // 執行 Bird move方法，輸出 fly

// 延伸應用：搭配陣列統一處理多型物件
Animal[] animals = { new Dog(), new Bird() };
for ( Animal animal : animals ) {
    animal.move();
}
```
