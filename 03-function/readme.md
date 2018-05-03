# 2-1. 호이스팅 (Hoisting)

변수 '선언'과 함수 '선언'을 끌어올림

    console.log(a());
    
    function a() {
        return 'a';
    }
    
# 2-2. 함수선언문과 함수표현식 (function declaration vs function expression)

    // 함수선언문 (function declaration)
    function a() {
        return 'a';
    }
    // 기명 함수표현식 (named function expression)
    var b = function bb() {
        return 'bb';
     }
     // (익명) 함수표현식 [(unnamed / annonymous) function expresson]
     var c = function() {
        return 'c';
     }
     
함수 표현식과 함수 선언문의 차이는 할당의 차

변수에 함수를 할당하게되면 변수만 호이스팅이 되게

협업 등의 문제로 인해 함수 표현식 대신 함수 표현식을 쓰는 것을 권장

예)

    var sum = function(a, b) {
        return a + ' + ' + b + ' = ' + (a + b);
    }
    sum(1, 2);
    
    /* ... 중략 ... */
    
    var sum = function(a, b) {
        return a + b;
    }
    sum(3, 4);
    
# 2-3. 함수스코프, 실행컨텍스트 (function scope, execution context)

스코프 : 유효범위(변수)

실행 컨텍스트 : 실행되는 코드 덩어리(추상적인 개념)

스코프는 정의될 때 결정됨!!

실행 컨텍스트는 실행될 때 생성됨!!

실행 컨텍스트에는 호이스팅, this 바인딩 등의 정보가 담김

    var a = 1;
    function outer() {
        console.log(a);
        
        function innter() {
            console.log(a);
            var a = 3;
        }
        
        inner();
        
        console.log(a);
    }
    outer();
    console.log(a);
    
# 2-4. 메소드(method)

함수처럼 생겼는데 .이 붙으면 메소드

    var obj = {
        a: 1,
        b: function bb() {
            console.log(this);
        },
        c: function() {
            console.log(this.a);
        }
    };
    
    obj.b();
    obj.c();
    
    console.log(obj.b);
    console.log(obj.c);
    
메소드는 this를 바인딩함

# 2-5. 콜백함수(callback function)

something will call this function back sometime somehow.

제어권을 넘겨준다. 맡긴다.

    setTnterval(function(){
        console.log('1초마다 실행될 겁니다.');
    }, 1000);
    
만약 콜백함수를 변수로 치환하면 아래와 같아집니다.

    var cb = function() {
        console.log('1초마다 실행될 겁니다.');
    };
    
    setTnterval(cb, 1000);
    
setInterval 함수는 아래와 같이 정의되어 있음
   
    setInterval(callback, milliseconds)
    
다른 예제

    var arr = [1, 2, 3, 4, 5];
    var entries = [];
    arr.forEach(function(v, i){
        entries.push([i, v, this[i]]);
    }, [10, 20, 30, 40, 50]);
    console.log(entries);
    
결과는 아래와 같이 나오게 됩니다.

    [[0,1,10],[1,2,20],[2,3,30],[3,4,40],[4,5,50]]
    
ForEach 구현

    Array.prototype.forEach = function(callback, thisArg) {
        var self = thisArg || this;
        for(var i = 0;i < this.length; i++) {
            callback.call(self,this[i], i, this)
        }
    }
    
콜백함수는 정해진 파라미터 규칙을 따라야함

    document.body.innerHTML = '<div id="a">abc</div>'
    function cbFunc(x) {
        console.log(this, x);
    }
    
    document.getElementById('a')
        .addEventListener('click', cbFunc);
        
    $('#a').on('click', cbFunc);
    
결과는 다음과 같이 나오게 됩니다.

    <div id="a">abc</div>
    MouseEvent {isTrusted:true, screenX:11, ...
    
addEventListener(type, callback, options)


## 콜백함수의 특징

- 다른 함수(A)의 매개변수로 콜백함수(B)를 전달하면, A가 B의 제어권을 갖게 됩니다.
- 특별한 요청(bind)이 없는 한 A에 미리 정해진 방식에 따라 B를 호출합니다.
- 미리 정해진 방식이란 this에 무엇을 바인딩할지, 매개변수에는 어떤 값들을 지정할지, 어떤 타이밍에 콜백을 호출할지 등이다.

주의! 콜백은 '함수'다