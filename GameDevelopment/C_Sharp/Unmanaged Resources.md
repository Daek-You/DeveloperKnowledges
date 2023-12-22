- Garbage Collection이 되지 않기에, 직접적으로 메모리 정리가 필요
- 파일, 창, 네트워크 연결, 데이터베이스 연결과 같은 OS 자원을 래핑하는 객체가 대표적
- 이와 관련하여 `System.IDisposable`, `Object.Finalize`, `GC.SuppressFinalize`와 같은 [레퍼런스](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged)를 참고해볼 것

