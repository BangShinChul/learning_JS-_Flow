# 클로저

A closure is the combination of a function and the lexical environment within which that function was declared.

클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다.

 lexical environment : 선언 당시의 환경에 대한 정보를 담은 객체 (구성 환경)
 
 함수와 "그 함수가 선언될 당시의 환경정보" 사이의 조합
 
 선언당시! SCOPE!
 
 함수 내부에 생성한 데이터와 그 유효범위로 인해 발생하는 특수한 현상 / 상태
 
closure : 닫혀있음. 폐쇄성. 완결성

return 외부에 정보를 제공할 수 있는 유일한 수단

return function : scope 및 lexical environment는 변하지 않는다.<br>
최초 선언시의 정보를 유지!

클로저를 잘 활용하면 아래와 같은 이점을 가질 수 있음<br>
- 접근 권한 제어
- 지역변수 보호
- 데이터 보존 및 활용

접근 권한 제어, 지역변수 보호

        function a() {
            var x = 1;
            function b()  {
                console.log(x); //  O
            }
            b();    //  O
        }
        a();    // X
        console.log(x);
    
데이터 보존 및 활용

    function a() {
        var x = 1;
        return function b() {
            console.log(x);
        }
    }
    var c = a();
    c();
    
접근 권한 제어, 데이터 보존 및 활용

    function a() {
        var _x = 1;
        return {
            get x() { return _x };
            set x(v) { _x = v; }
        }
    }
    var c = a();
    c.x = 10;
    
예제

    function setName(name) {
        return function() {
            return name;
        }
    }
    var sayMyName = setName('고무곰');
    sayMyName();
    
0. 전역 실행컨텍스트 생성[ GLOBAL ]<br>
1. 함수 setName 선언 [ GLOBAL > setName ]<br>
2. 변수 sayMyName 선언<br>
3. setName('고무곰') 호출 -> setName 실행컨텍스트 생성<br>
4. 지역변수 name 선언 및 '고무곰' 할당<br>
5. 익명함수 선언 [ GLOBAL > setName > unnamed ]<br>
6. 익명함수 반환<br>
7. setName 실행컨텍스트 종료<br>
8. 변수 sayMyName에 반환된 함수를 할당<br>
9. sayMyName 호출 -> sayMyName 실행컨텍스트 생성<br>
10. unnamed scope에서 name탐색 -> setName에서 name탐색 -> '고무곰'<br>
11. sayMyName 실행컨텍스트 종료<br>
12. 전역실행컨텍스트 종료

스코프는 정의될 때 결정된다!!

    function setCounter() {
        var count = 0;
        return function() {
            return ++count;
        }
    }
    var count = setCounter();
    count();
    
1. setCounter 정의 [ GLOBAL > setCounter ]
2. setCounter 실행
3. setCounter 스코프에 counter 변수 선언 및 0 할당
4. 익명함수 정의 및 반환 [ GLOBAL > setCounter > 익명 ]
5. 반환된 익명함수를 변수 count에 할당
6. count 실행
7. 익명함수 스코프에서 count 탐색 -> setCounter 스코프에서 count 탐색 -> count에 1을 증가시킨 값을 반환

# 지역변수 만들기

closure로 private member 만들기

    var car = {
        fuel: 10,   // 연료 (l)
        power: 2,    // 연료 ( km / l )
        total: 0,
        run: function(km) {
            var wasteFuel = km / this.power;
            if(this.fuel < wasteFuel) {
                console.log('이동 불가');
                return;
            }
            this.fuel -= wasteFuel;
            this.total += km;
        }
    };
    
변경

    var createCar = function (f, p) {
        var fuel = f;
        var power = p;
        var total = 0;
        return {
            run : function(km) {
                var wasteFuel = km / power;
                if( fuel < wasteFuel ) {
                    console.log('이동 불가');
                    return;
                }
                fuel -= wasteFuel;
                total += km;
            }
        }
    };
    var car = createCar(10, 2);
    
1. 함수에서 지역변수 및 내부함수 등을 생성한다.
2. 외부에 노출시키고자 하는 멤버들로 구성된 객체를 return한다. -> return한 객체에 포함되지 않는 멤버들은 private하다. -> return한 객체에 포함된 멤버들은 public하다

return function 최초 선언시의 정보를 유지!

접근 권한 제어, 지역변수 보호, 데이터 보존 및 활용