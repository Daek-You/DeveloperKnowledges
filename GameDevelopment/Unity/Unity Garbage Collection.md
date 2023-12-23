
## Unity의 기존 GC 방식

- Unity는 Boehm-Demers-Weiser 가비지 컬렉션 방식을 사용 (Mark ans Sweep 사용)
	- 허나 세대별 GC, Compaction 적용은 하지 않는 구시대 방식
- 이는 Stop-the-world 방식으로, 가비지 컬렉션을 수행해야 할 때마다 실행 중인 프로그램을 중지했다가 작업이 모두 완료된 후에야 실행을 재개
- 이는 게임과 같은 실시간 상호작용을 하는 프로그램에 큰 영향을 미칠 수 있음
- 프로파일러에서 GC Spike 관찰 가능

## 점진적 가비지 컬렉션(Incremental Garbage Collection)
#Unity_19_1a10  

- 점진적 GC 또한 Boehm-Demers-Weiser 방식을 사용하나, 작업 부하를 여러 프레임에 분할하여 진행 👉🏻 GC Spike가 줄어듬  
- 여러 프레임에 걸쳐 나누어 진행하기에, 애플리케이션의 실행을 더 짧게 중단하는 장점을 가짐
