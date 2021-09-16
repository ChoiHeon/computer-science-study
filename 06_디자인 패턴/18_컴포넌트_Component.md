# 컴포넌트 패턴

* 한 개체가 여러 분야를 서로 커플링 없이 다룰 수 있게 합니다.
* 로직을 기능별로 컴포넌트화해서 클래스로 분리하는 것입니다.
  * 가독성을 올리고 디커플링을 시키는 것
* 특정 문제를 해결하기 위해 여러 분야를 알아야만 하는 경우를 방지합니다. 
  * 즉, 디커플링을 위한 패턴입니다.
  * 커플링이란 모듈간의 의존성의 정도를 의미합니다.
* 예를 들어서 물리 프로그래머는 그래픽 처리를 신경쓰기 않고 자기 코드를 수정해야 하고, 반대 역시 마찬가지입니다.
* 따라서 빠른 유지보수가 가능합니다.
* 유니티는 이러한 컴포넌트 구조를 사용하기 용이합니다.



### Example

* 한 코드내에서 오브젝트의 회전, 이동, 0.5초 대기를 구현할 경우, 수정하기 불편함

```C#
public class GameMamager : Monobehavior {
    GameObject player1;
    GameObject plater2;
    Move move - Move.RIGHT;
    
    void Start() {
        plater1 = GameObject.Find("Player1") as GameObject;
        plater2 = GameObject.Find("Player2") as GameObject;
        
        StartCouroutine("Act");
    }
    
    IEnumerator Act() {
        while (true) {
            player1.transform.Rotate(90.0f * Vector3.up);
            player2.transform.Rotate(90.0f * Vector3.up);
            
            if (player1.transform.position.x < -4)
            {
                move = MOVE.MOVE_RIGHT;
            }
            else if (player1.transform.position.x > 4)
            {
                move = MOVE.MOVE_LEFT;
            }

            if (move == MOVE.MOVE_RIGHT)
            {
                player1.transform.Translate(1.0f * Vector3.right, Space.World);
                player2.transform.Translate(-1.0f * Vector3.right, Space.World);
            }
            else
            {
                player1.transform.Translate(-1.0f * Vector3.right, Space.World);
                player2.transform.Translate(1.0f * Vector3.right, Space.World);
            }

            yield return new WaitForSeconds(0.5f);
        }
    }
}
```

<br>

* 이럴 때 컴포넌트 패턴을 사용해서 기능을 분리할 필요가 있습니다.

```c#
class MoveA :Monobehavior {
    MOVE2 move = MOVE2.MOVE_RIGHT;

    void Start () {
        StartCoroutine("Move");
    }

    // 0.5초마다 (글로벌 방향으로) 이동시키기
    IEnumerator Move()
    {
        while (true)
        {
            if (transform.position.x < -4)
            {
                move = MOVE2.MOVE_RIGHT;
            }
            else if (transform.position.x > 4)
            {
                move = MOVE2.MOVE_LEFT;
            }

            if (move == MOVE2.MOVE_RIGHT)
            {
                transform.Translate(1.0f * Vector3.right, Space.World);
            }
            else
            {
                transform.Translate(-1.0f * Vector3.right, Space.World);
            }

            yield return new WaitForSeconds(0.5f);
        }
    }
}
```

```c#
public class RotateAct : MonoBehaviour {

	void Start () {
        StartCoroutine("Rotate");
    }

    // 0.5초마다 객체 회전시키기
    IEnumerator Rotate()
    {
        while (true)
        {
            transform.Rotate(90.0f * Vector3.up);

            yield return new WaitForSeconds(0.3f);
        }
    }
}
```





