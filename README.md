# Garbage-Collection
가비지 컬렉션정리

## 가비지 컬렉션 (Garbage Collection)
- 프로그램에서 사용하지 않는 동적으로 할당된 메모리를 자동으로 해제하는 메모리 관리 기법입니다.
- 
- GC는 프로그래머가 명시적으로 메모리를 할당하거나 해제하는 번거로움과 실수를 줄이고, 메모리 누수와 같은 일반적인 메모리 관련 문제를 방지합니다.

### 가비지 컬렉션의 필요성

- 자동 메모리 관리: 프로그래머는 객체를 생성하고 사용하는 것에 집중할 수 있으며, 할당된 메모리의 해제를 신경쓰지 않아도 됩니다. 이로 인해 개발 생산성이 향상됩니다.

- 메모리 누수 방지: GC는 사용하지 않는 객체들을 감지하고 자동으로 해제합니다. 이로 인해 메모리 누수와 관련된 문제를 방지할 수 있습니다.

### 가비지 컬렉션의 동작 매커니즘

- Reachability(도달 가능성) 검사: GC는 도달 가능한 객체를 찾기 위해 루트(root) 객체들을 확인합니다. 루트 객체는 일반적으로 실행 중인 스레드의 스택, 정적 객체, 전역 변수 등입니다.

- Marking(표시): 도달 가능한 객체들을 표시(mark)합니다. 즉, 이들 객체들은 유효하다고 표시됩니다.
 
- Sweeping(해제): 표시되지 않은(non-marked) 객체들은 사용되지 않는 객체로 간주되어 해제됩니다. 이 과정에서 메모리가 반환됩니다.

- 정리 및 압축: GC는 메모리를 조각화되지 않은 상태로 유지하기 위해 사용 중인 메모리 영역을 정리하고, 객체들을 연속적인 메모리 영역으로 이동시킵니다.

### 가비지 컬렉션의 동작 매커니즘 예제
```C#
using System;

class Program
{
    static void Main()
    {
        // 객체 생성
        MyClass obj1 = new MyClass();
        MyClass obj2 = new MyClass();
        
        // obj2가 obj1을 참조
        obj2.Reference = obj1;
        
        // obj1을 null로 설정하여 참조 해제
        obj1 = null;
        
        // GC 실행을 알리기 위해 가비지 수집 요청
        GC.Collect();
        
        // GC가 실행되기 위해 잠시 대기
        GC.WaitForPendingFinalizers();
        
        // obj2가 GC에 의해 수집되지 않았는지 확인
        Console.WriteLine(obj2.Reference != null);  // true
        
        // obj2도 null로 설정하여 참조 해제
        obj2 = null;
        
        // GC 실행을 알리기 위해 다시 가비지 수집 요청
        GC.Collect();
        
        // GC가 실행되기 위해 잠시 대기
        GC.WaitForPendingFinalizers();
        
        // obj2도 GC에 의해 수집되었는지 확인
        Console.WriteLine(obj2 == null);  // true
    }
}

class MyClass
{
    public MyClass Reference { get; set; }
    
    ~MyClass()
    {
        Console.WriteLine("Finalizer가 호출되었습니다.");
    }
}
```

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
