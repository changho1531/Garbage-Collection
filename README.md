# Garbage-Collection
가비지 컬렉션정리

## 가비지 컬렉션 (Garbage Collection)

- GC에 대해 조사하고, 본인의 깃허브에 public 프로젝트로 등록하여 정리하시오.
- 왜 필요한지, 어떤 매커니즘으로 동작하는지, 사용 방법에 대해 설명하시오.

### 가비지 컬렉션의 필요성

- 가비지 컬렉션은 메모리 관리의 편의성과 메모리 누수 방지를 위해 필요합니다.

### 가비지 컬렉션의 동작 매커니즘

- 가비지 컬렉션은 Reachability 판별과 Mark and Sweep 등의 기본 매커니즘으로 동작합니다.

### 가비지 컬렉션의 사용 방법

- C#에서 가비지 컬렉션은 자동으로 수행되므로 개발자의 개입이 필요하지 않습니다. 그러나 성능 최적화와 메모리 누수 방지를 위해 몇 가지 사항에 주의해야 합니다.

### 제대로 동작하는 가비지 컬렉션을 위한 코드 작성 방법

- 긴 생명 주기 객체의 명시적 해제
- 대규모 객체 사용 최소화
- 참조 관리에 주의

### 가비지 컬렉션으로 인한 메모리 누수 발생 예제

- 코드 예제와 함께 가비지 컬렉션으로도 메모리 누수가 발생하는 상황을 설명합니다.

```csharp
class MyClass
{
    private SomeResource resource;

    public MyClass()
    {
        resource = new SomeResource();
    }

    public void DoSomething()
    {
        // resource 사용
    }
}

// 사용 예시
void Main()
{
    MyClass myObject = new MyClass();
    myObject.DoSomething();

    // myObject을 사용한 후에 참조 해제하지 않음

    // 가비지 컬렉션을 수행하도록 요청
    GC.Collect();
    GC.WaitForPendingFinalizers();
}
```
