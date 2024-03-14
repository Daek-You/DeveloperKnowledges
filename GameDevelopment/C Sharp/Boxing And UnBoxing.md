### Boxing

- **`값 형식(Value type)`** 을 **`참조 형식(Reference Type)`** 으로 변환하는 작업
- C#의 모든 클래스들은 **`object`** 라는 클래스에서 파생되었기에 가능한 작업

```csharp
int i = 10;
object o = i;   // Boxing
```  

- 참조 타입처럼 **`힙 메모리`** 에 새로 할당되고 거기에 값을 복사해 넣음
- 여러 타입의 자료형들을 배열 등으로 관리할 때 유용
- 힙 메모리 할당이므로 무분별한 Boxing 연산은 **`GC`** 초래 위험이 있음  

### UnBoxing

- **`Boxing한 객체가 가리키는 값`** 을 다시 **`값 형식(Value type)`** 에 복사해 넣는 작업  

```csharp
int j = (int)o;
```


## 결론

- Boxing과 Unboxing은 특별한 경우를 제외하곤, 성능 상 사용하지 않는 것을 권장
- 대신 **`Generic`** 을 사용