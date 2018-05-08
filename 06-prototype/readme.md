# 5-1. prototype과 constructor, proto

![Alt text](prototype.png)

![Alt text](prototype2.png)

왼쪽에는 생성자, 오른쪽에는 생성자의 프로토타입, 아래에는 인스턴스

![Alt text](prototype_ex.png)

![Alt text](prototype_ex2.png)

    console.dir([1, 2, 3]);
    
    // Array(3) : 0: 1 , 1: 2, 2: 3, length : 3, __proto__: Array(0)
    
프로토타입 예제

    function Person(n, a) {
        this.name = n;
        this.age = a;
    }
    var gomu = new Person('고무곰', 30);
    
    var gomuClone1 = new gomu.__proto__.constructor('고무곰_클론1', 10);
    
    var gomuClone2 = new gomu.constructor('고무곰_클론2', 25);
    
    var gomuProto = Object.getPrototypeOf(gomu);
    var gomuClone3 = new gomuProto.constructor('고무곰_클론3', 20);
    
    var gomuClone4 = new Person.prototype('고무곰_클론4',15);
    
    
    [CONSTRUCTOR].prototype

    [instance].__proto__
    
    [instance]
    
    Object.getPrototypeOf([instance])
    
위의 네가지함수를 통해서 생성자함수의 프로트타입에 접근이 가능함.

    [CONSTRUCTOR]
    
    [CONSTRUCTOR].prototype.constructor
    
    (Object.getPrototypeOf([instance])).constructor
    
    [instance].__proto__.constructor
    
    [instance].constructor
    
위의 다섯가지방법으로 생성자 함수에 접근가능

# 5-2. 메소드 상속 및 동작 원리

    function Person(n, a) {
        this.name = n;
        this.age = a;
    }
    
    var gomu = new Person('고무곰', 30);
    var iu = new Person('아이유', 25);
    
    gomu.setOlder = function() {
        this.age += 1;
    }
    gomu.getAge = function() {
        return this.age;
    }
    iu.setOlder = function() {
        this.age += 1;
    }
    iu.getAge = function() {
       return this.age;
    }
   
프로토타입으로 변경 후

    function Person(n, a) {
        this.name = n;
        this.age = a;
    }
    Person.prototype.setOlder = function() {
        this.age += 1;
    }
    Person.prototype.getAge = function() {
        return this.age;
    }
    
    var gomu = new Person('고무곰', 30);
    var iu = new Person('아이유', 25);
    
    Person.prototype.age = 100;
    gomu.__proto__.setOlder();
    gomu.__proto__.getAge();    // 101
    
    gomu.setOlder();
    gomu.getAge();  // 31