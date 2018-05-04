# this

## 전역공간에서 : window / global

    /* 브라우저 콘솔에서 */
    console.log(this); -> window
    
    /* node.js에서 */
    console.log(this); -> global

## 함수 내부에서 : window / global (기본적으로! 변경가능)

## 메소드 호출시 : 메소드 호출 주체 (메서드명 앞)

함수는 (전역객체의) 메소드다!

## callback에서 : 기본적으로 함수내부에서와 동일

call, apply, bind 차이

func.call(thisArg[, arg1[, arg2[, ...]]]) <br>
func.apply(thisArg, [argsArray]) <br>
func.bind(thisArg[, arg1[, arg2[, ...]]])

call, apply의 차이는 두번째 파라미터를 어떻게 받는냐의 차이

call,apply와 bind의 차이는 call,apply는 즉시 호출하며 bind는 새로운 함수 생성(currying)을 합니다.

### 정리

- 기본적으로 함수의 this와 같다
- 제어권을 가진 함수가 callback의 this를 명시한 경우 그에 따른다.
- 개발자가 this를 바인딩한 채로 callback을 넘기면 그에 따른다.

생성자함수에서 : 인스턴스