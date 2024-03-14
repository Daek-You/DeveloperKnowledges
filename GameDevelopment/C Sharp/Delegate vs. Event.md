
### Delegate

- C++의 함수 포인터와 유사한 개념
	- 허나, **`Delegate`** 는 `객체에 대한 주소` + `메서드 주소` 를 함께 가지고 있다는 차이가 있음
- 메서드의 매개변수와 리턴 타입에 대해 정의 후, 동일한 형태의 메서드를 담을 수 있음
- 함수를 인자로 받아, 여러 상황 별로 코드를 중복해 작성하는 것을 방지할 수 있음
- **`콜백(Callback)`** 처리 하는 용도에 주로 사용  

```csharp
// int형 매개변수 2개를 받고, int 타입을 리턴하는 delegate 선언
delegate int Function(int param1, int param2); 
```  

```csharp
public int Calculator(Function mode, int param1, int param2)
{
	return mode(param1, param2);
}
```  

```csharp

public static int Add(int x, int y)   { return x + y; }
public static int Minus(int x, int y) { return x - y; }

public static void Main(string[] args)
{
	Function addFunc = new Function(Add);
	Function minusFunc = new Function(Minus);

	int x = 10, y = 5;
	int result1 = Calculator(addFunc, x, y);    // 15
	int result2 = Calculator(minusFunc, x, y);  // 5
}
```  


### Event

- 특수한 형태의 Delegate이며, Delegate가 가지는 위험한 부분들을 보완하기 위해 등장  

```csharp
delegate int Function(int param1, int param2);
public event Function funcEvent;
```

> [!warning] Delegate를 사용할 때 위험한 상황 #1 : **`= 연산자`**  
> Delegate에 많은 함수들이 체이닝(Chaining) 된 상황에서, 프로그래머가 실수로 해당 델리게이트에 **`= 연산자`** 를 사용하여 함수를 등록했다면? 이전에 등록해놨던 모든 함수들이 날아가버린다.  
> 
> **`Event`** 에서는 이와 같은 일이 발생하지 않도록, **`= 연산자`** 사용을 막아놓았다.  

> [!warning] Delegate를 사용할 때 위험한 상황 #2 : **`외부 호출`**  
> Delegate는 선언된 클래스 내부 뿐만 아니라, 외부에서도 편하게 호출할 수 있다. 주로 콜백 용도로 사용되는 Delegate를 외부에서 무작위로 호출이 가능하다면, 시스템에 혼란을 초래할 수 있다.  
> 
> 이를 방지하기 위해 **`Event`** 는 **오로지 클래스 내부에서만 호출**할 수 있으며, public으로 선언해도 외부에서 접근이 불가능하다.  

- 이러한 특징에 의해, **`Event`** 는 **객체 내의 상태가 변경되었다는 이벤트 발생 용도**로 주로 사용  

<br>

## C#에서 제공하는 기본 Delegate

- **`Action<T>`** 함수 매개변수 타입이 T 이고, 리턴 타입이 void 인 경우
```csharp
Action<string> Print = (string parameter) => Console.WriteLine(parameter);
```  

- **`Func<T, TResult>`** 함수 매개변수 타입이 T 이고, 리턴 타입이 TResult 인 경우
```csharp
Func<string, string> GetPrint = (string parameter) => parameter;
```  

- **`Predicate<T>`** 함수 매개변수 타입이 T 이고, 리턴 타입이 bool 인 경우
```csharp
Predicate<int> checkNumber = (int num) => num > 10;
```



