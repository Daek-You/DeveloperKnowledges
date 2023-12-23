
# CLR(Common Language Runtime)

- .NET 애플리케이션 실행을 관리하는 Microsoft .NET Framework 구성 요소
- Java의 VM(Virtual Machine)과 비슷한 역할을 진행
- .NET 언어로 작성된 소스 코드를 OS 위에 있는 .NET Framework에서 작동하게 해주는 역할  

![Architecture](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc14jyH%2FbtqDNoaxbt3%2F5KRpBCMqFkKS4PHERkEnSk%2Fimg.png)
<br>

## 내부 구조

![내부 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2g60y%2FbtqDL3ZgCmo%2FHfh88r4e6PE65kkFvHJPxk%2Fimg.png)  

- `Base Class Library Support(BCL)`
  Collections, I/O, XML, DataType 정의 등을 지원
- `Thread Support`
  멀티 스레드의 병렬 실행을 관리하기 위한 기능 제공
- `Com Marshaller`
  COM(Component Object Model) 기술을 통해, 프로그램이나 시스템을 이루는 컴포넌트들이 상호 통신을 할 수 있게 만들어 주는 역할
- `Type Checker`
  CLR 내부의 CTS(Common Type System)과 CLS(Common Language Specification)를 사용하여 형식을 검사
- `Exception Manager`
  .NET 언어에 상관없이 예외 처리 적용
- `Security Engine`
  .NET Framework에서 제공되는 툴을 이용하여 Code, Folder, Machine Level 단위로 보안 사용 권한 처리
- `Debug Engine`
  응용프로그램이 런타임 동안 디버그가 가능하게 만들어 줌. 이와 관련하여 코드 관리 및 추적에 편한 ICorDebug 인터페이스가 존재
- `JIT Compiler`
  CIL(Common Intermediate Language)를 실행되는 컴퓨터 환경에 맞는 기계어 코드로 변환. 컴파일된 CIL은 필요한 경우, 후속 호출에 사용할 있도록 저장됨
- `Code Manager`
  [**Managed code**](Managed%20code와%20Unmanaged%20code.md) 관리
- [**Garbage Collector**](Garbage%20Collection.md)
  자동 메모리 관리 역할을 하며, 더 이상 필요하지 않은 메모리 공간을 자동으로 해제하여 재할당이 가능하다록 한다.
- `CLR Loader`
  다양한 모듈, 리소스, 어셈블리 등은 CLR Loader에 의해 로드된다. 또한, 프로그램의 초기화 시간이 더 빠르고 소모되는 자원이 덜 필요한 경우, CLR Loader는 모듈을 On-Demand 방식으로 로드
  - On-Demand 방식이란 요구가 있을 때 해당 내용을 적용하겠다는 의미
<br>
# 동작 원리

![동작 원리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKqSsI%2FbtqDLESTXjo%2Fiey4K0KK2d8xSpt8iGJZWK%2Fimg.jpg)  
  
1. .NET 언어로 소스 코드를 작성하면, 해당 언어 컴파일러가 **`CIL(Common Intermediate Language)`** 로 작성된 실행 파일 생성 (위 그림에선 **`MSIL(Microsoft Intermediate Language)`** )
2. 사용자가 해당 파일을 실행하면, CLR이 CIL을 읽어 JIT 컴파일을 진행하여 네이티브 코드로 변환하여 실행

- 장점
  플랫폼에 최적화된 코드를 만들어 냄.
- 단점
  실행 시 이루어지는 컴파일 비용의 부담이 커짐.
<br>



![CLR](https://media.geeksforgeeks.org/wp-content/uploads/Overview-of-the-.NET-Framework-min.png)  