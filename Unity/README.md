
## 목차

- [프레임워크](#프레임워크)
- [라이프사이클](#유니티-라이프사이클)
- [GetComponent](#getcomponent)

---

## 프레임워크
GameManager는 게임 전체의 흐름과 상태를 관리하는 중앙 관리자이다. 게임 안에 하나만 존재하도록 하기 위해 Singleton 패턴으로 만든다. 
- SaveManager
- SceneManager
- TableManager
- AudioManager
...

## 유니티 라이프사이클

- Awake()← 컴포넌트 초기화 (캐싱) 
- OnEnable() ← 오브젝트 활성화될 때 
- Start() ← 첫 프레임 전 (다른 오브젝트 참조)
- FixedUpdate() ← 물리 연산 (일정 간격)
- Update() ← 매 프레임 (입력, 이동)
- LateUpdate() ← Update 후 (카메라)
- OnDisable() ← 오브젝트 비활성화될 때 
- OnDestroy() ← 오브젝트 삭제될 때

```c#
public class Player : MonoBehaviour
{
    private Rigidbody rb;

    void Awake()
    {
        // 가장 먼저 실행 - 컴포넌트 캐싱
        rb = GetComponent<Rigidbody>();
    }

    void Start()
    {
        // Awake 이후 - 초기값 설정
        rb.mass = 1f;
    }

    void FixedUpdate()
    {
        // 물리 연산 - 일정한 간격으로 실행 (기본 0.02초)
        rb.AddForce(Vector3.up);
    }

    void Update()
    {
        // 매 프레임 실행 - 입력 감지
        if (Input.GetKeyDown(KeyCode.Space))
        {
            rb.AddForce(Vector3.up * 10f);
        }
    }

    void LateUpdate()
    {
        // Update 끝난 후 실행 - 카메라 추적
        Camera.main.transform.position = transform.position;
    }

    void OnDestroy()
    {
        // 오브젝트 삭제될 때 - 정리 작업
        Debug.Log("Player 삭제됨");
    }
}
```

## GetComponent
유니티 게임 오브젝트에는 여러 컴포넌트(Transform, RigidBody, SpriteRenderer 등)들이 있다. GetComponent는 오브젝트에 붙어있는 특정 부품을 찾아온다.

```c#
void Start () 
{
    Rigidbody rb = GetComponent<Rigidbody>();
}
```
또 다른 방법으로는 인스펙터 창에서 직접 드래그 앤 드롭으로 연결하는 것이다.

```c#
public class Player : MonoBehaviour
{
    public Rigidbody rb;
}    
```

주로 자기 자신에게 붙은 컴포넌트나 런타임에 동적으로 생성되는 오브젝트를 다룰 때 GetComponent를 사용하고,
플레이어 스크립트, 체력바UI, 카메라, OnCollisionEnter(부딪힌 상대방을 누군지 알 수 없다)의 경우 인스펙터 창에서 직접 할당한다.

## 관련 용어들

> 캐싱 : 자주 쓰는 컴포넌트나 오브젝트를 변수에 미리 저장해두는 것
