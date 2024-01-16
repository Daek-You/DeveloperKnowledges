### Fake Null이란?

- Unity 엔진은 C++로 작성되어 있으며, 이를 우리는 C# 클래스로 래핑(Wrapping)하여 사용
- **`UnityEngine.Object`** 가 바로, C++로 작성된 네이티브 객체의 Wrapper 클래스
- 이러한 네이티브 객체는 **`1)씬이 변경`** 되거나, **`2)Destroy()`** 를 할 경우 삭제됨
- 하지만 네이티브 객체를 래핑했던 **C# 오브젝트는 아직 GC를 기다리는 중**
- 이렇게, 실제 객체는 사라졌지만 그 껍데기는 아직 사라지지 않은 상황을 **`Fake null`** 이라고 함  

<br>

이러한 구조 덕분에 **`UnityEngine.Object`** 의 **`Null`** 유무를 확인할 때 주의가 필요하다.  

> [!tip] `==` 연산자로 비교 (안전)  
> - `UnityEngine.Object`는 내부적으로 `==`, `!=` 연산자가 오버로딩 되어 있음  
> - `UnityEngine.Object`의 `==` 연산자는 C# 래핑 부분 뿐만 아니라, 실제 네이티브 객체도 존재하는 지를 확인함  
> - null 체크를 둘 다 하기에 성능이 좀 느리지만, `UnityEngine.Object`가 파괴될 수 있는 상황이라면 안전한 `==` 연산자가 나음  

> [!warning] `is`, `ReferenceEquals()`, `Equals()`  (불안전)    
> - C++ 네이티브 객체의 실존 여부를 판단하지 않고, 값만 판단
> - `System.Object`에 정의되어 있어, `UnityEngine.Object`의 `==` 오버로딩을 따르지 X
> - null check 비교 과정이 생략되어 속도가 빠름
> - 다만, Fake Null 체크를 하지 못해 위험할 수 있음    

- 결론은 **`UnityEngine.Object`** 는 `C# Null 관련 문법`으로 체크할 시 주의 필요  
> [!error] **`is`** , **`Equals`** , **`ReferenceEquals`** , **`? 연산자`**  


