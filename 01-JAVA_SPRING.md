## Java, Spring

<details>
  <summary><h3>1. JVM이 정확히 무엇이고, 어떤 기능을 하는지 설명해 주세요.</h3></summary>
    <details>
      <summary>답변</summary>
      <p>시스템 메모리를 관리하면서, 자바 기반 애플리케이션을 위해 이식 가능한 실행 환경을 제공합니다.</p>
      <p>기능은 크게 2가지로 첫 번째로 자바 프로그램이 어느 기기나 OS 상에서도 실행될 수 있도록 하는 것이며 두 번째는 프로그램 메모리를 관리하고 최적화하는 것입니다.</p>
      <details>
        <summary>꼬리 질문</summary>
        <ul>
        <li> 그럼 JVM은 어떤 실행 과정을 거치나요? 
          <details>
            <summary>답변</summary>
            <p>1. 프로그램이 실행되면, JVM은 OS로부터 메모리를 할당 받고 용도에 따라 여러 영억으로 나누어 관리합니다.</p>
            <p>2. 자바 컴파일러가 소스코드를 읽고, 바이트코드(.class)로 변환 시킵니다.</p>
            <p>3. 변경된 클래스 파일들을 클래스 로더를 통해 JVM 메모리 영역으로 로딩합니다.</p>
            <p>4. 로딩된 클래스 파일들은 Execution engine을 통해 해석됩니다.</p>
            <p>5. 해석된 바이트 코드는 메모리 영역에 배치되어 실질적인 수행이 이루어집니다. 이런 실행 과정 속 JVM은 필요에 따라 스레드 동기화나 가비지 컬렉션 같은 메모리 관리 작업을 수행합니다.</p>
          </details>
        </li>
        <li> 가비지 컬렉션이 뭔지 설명해 주세요.
          <details>
            <summary>답변</summary>
            <p>가비지 컬렉션은 Heap 영역에 있는 데이터들 중 불필요해진 데이터들을 자동으로 정리해주는 작업입니다.</p>
            <p>실행 순서는 참조되지 않은 객체들을 탐색 후 삭제, 삭제된 객체의 메모리 반환 그리고 힙 메모리 재사용의 순서로 실행됩니다.</p>
          </details>
        </li>
        <li> 가비지 컬렉션의 Heap 영역의 구조에 대해 설명해 주세요.
          <details>
            <summary>답변</summary>
            <img src = "resources/JVM_Heap.png">
            <p>Heap 영역의 경우 크게 Young Generation과 Old Generation으로 나뉘어집니다.</p>
            <p>인스턴스를 처음 생성하면 Young 영역의 메모리가 배정됩니다. 특히 그 중 맨 처음에는 Eden 영역에 할당이 됩니다. Eden 영역이 꽉 차서 더이상 할당이 불가능 하면 Minor GC에 의해 Eden 영역에 있는 요소의 참조 여부를 살피고 GC를 진행합니다. 이렇게 Young 영역에서 발생하는 GC를 Minor GC라고 부릅니다.</p>
            <p>Minor GC를 통해 정리된 데이터들은 Survivor Space로 이동됩니다. Eden 영역과 S0, S1 영역중 하나에 대한 Minor GC의 결과물을 Minor GC가 진행되지 않은 Survivor Space에 옮겨놓고 기존 영역들을 정리하는 방식으로 S0, S1 영역중 하나의 영역에만 데이터가 존재하게 됩니다.</p>
            <p>반복되는 과정 속에서 S0, S1 영역을 반복적으로 이동하는 데이터가 있을 수 있는데, 이런 이동을 거치며 age값이 증가하고, 특정 값 이상이 되면 Old Generation으로 데이터가 옮겨지게 됩니다. 이렇게 Young 영역에서 Old 영역으로 데이터가 옮겨지는 것을 Promotion이라고 합니다.</p>
            <p>반복되는 Promotion으로 Old 영역에 데이터가 쌓이게 되고, Eden 영역과 같은 방식으로 GC를 통해 불필요한 요소를 제거하는 것을 Major GC라고 합니다.</p>
          </details>
        </li>
        <li> 그럼 어떤 방식으로 GC 대상을 파악하나요?
          <details>
            <summary>답변</summary>
            <p>기본적으로 Stop-The-World와 Mark & Sweep 알고리즘에 기초를 두고 있습니다.</p>
            <p>먼저 GC를 하기 위해 모든 스레드를 중단시키는데, 이를 Stop-The-World라 칭합니다.</p>
            <p>모든 스레드를 멈추고, 스택 내의 모든 지역 변수를 스캔하며 각각 어떤 오브젝트를 참조하고 있는지 찾는 과정이 Marking이고, 참조 되어있지 않은 오브젝트들을 Heap에서 제거 즉, Sweep 하게 됩니다. 이 방식을 Mark & Sweep이라 부릅니다.</p>
          </details>
        </li>
        <li> 자바 말고 다른 언어는 JVM 위에 올릴 수 없나요?
          <details>
            <summary>답변</summary>
            <p>자바 이외의 언어도 JVM에 올릴 수 있습니다. 예를 들어 Kotlin, Scala, Groovy 등의 언어가 있습니다.</p>
          </details>
        </li>
        <li> VM을 사용함으로써 얻을 수 있는 장점과 단점은 무엇인가요?
          <details>
            <summary>답변</summary>
            <p>장점으로는 플랫폼 독립성, 메모리 관리, 예외 처리, 다양한 라이브러리가 있습니다.</p>
            <p>단점으로는 바이트 코드를 해석하고 실행하는 과정에서 성능저하가 발생할 수 있고, 프로그램 실행에 필요한 메모미를 동적으로 할당하고 해제하기 때문에 일부 시스템에서는 메모리 사용량에 부담이 가해질 수 있습니다.</p>
          </details>
        </li>
        <li> JVM과 내부에서 실행되고 있는 프로그램은 부모 프로세스 - 자식 프로세스 관계를 갖고 있다고 봐도 무방한가요?
          <details>
            <summary>답변</summary>
            <p>JVM은 OS 위에서 실행되는 프로그램이며, 내부에서는 스레드를 사용하여 동시에 여러 작업을 처리하기 때문에 부모 - 자식 프로세스 관계와는 다릅니다.</p>
          </details>
        </li>
        </ul>
      </details>
    </details>
</details>

<details>
  <summary><h3>2. final 키워드를 사용하면, 어떤 이점이 있나요?</h3></summary>
    <details>
      <summary>답변</summary>
      <p>1. 변경 가능성을 최소화하여 예측 가능한 코드가 가능해집니다.</p>
      <p>2. thread-safe하여 따로 동기화 할 필요가 없습니다.</p>
      <p>3. setter-safe하여 외부에서 동적으로 값을 변경할 가능성이 줄어듭니다.</p>
      <p>4. 런타임 시 JVM의 최적화를 통해 성능상 사소한 이점을 얻을 수 있습니다.</p>
    </details>
</details>
