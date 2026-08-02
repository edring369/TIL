
## 목차

- [프레임워크](#프레임워크)
- [라이프사이클](#유니티-라이프사이클)

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
