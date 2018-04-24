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
    
# 함수스코프, 실행컨텍스트 (function scope, execution context)

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
    
