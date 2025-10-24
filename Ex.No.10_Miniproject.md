# Ex.No: 10  Implementation of 2D/3D Game
### DATE: 24/10/25
### REGISTER NUMBER: 212223240181

---

## AIM:
To develop a **2D chase-and-escape game** in Unity using an **AI-based enemy** that intelligently follows the player.

---

## ALGORITHM:
1. Start Unity and create a new 2D project named **“ChaseEscapeAI”**.  
2. Design the game scene by adding **Player** and **Enemy** sprites.  
3. Assign **Collider2D** and **Rigidbody2D** components to both Player and Enemy.  
4. Attach a **PlayerMovement** script to control movement using **WASD/Arrow keys**.  
5. Attach an **EnemyAI** script to make the enemy chase the player using simple AI logic.  
6. Implement **collision detection** in the Player script to trigger *Game Over* when the enemy touches the player.  
7. Test the game by running and adjusting object positions and speeds.  
8. Verify that the AI successfully chases the player and that collision detection ends the game.

---

## PROGRAM:

### **PlayerMovement.cs**
```csharp
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 5f;
    private Rigidbody2D rb;
    private Vector2 movement;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        movement.x = Input.GetAxisRaw("Horizontal");
        movement.y = Input.GetAxisRaw("Vertical");
    }

    void FixedUpdate()
    {
        rb.MovePosition(rb.position + movement * moveSpeed * Time.fixedDeltaTime);
    }

    void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Enemy"))
        {
            Debug.Log("Game Over!");
            Time.timeScale = 0;
        }
    }
}
```

### **EnemyAI.cs** ###
```csharp

using UnityEngine;

public class EnemyAI : MonoBehaviour
{
    public Transform player;
    public float speed = 3f;
    public float chaseRange = 6f;
    public float patrolSpeed = 2f;
    private Vector2 patrolTarget;

    private enum State { Patrol, Chase }
    private State currentState = State.Patrol;

    void Start()
    {
        ChooseNewPatrolPoint();
    }

    void Update()
    {
        float distance = Vector2.Distance(transform.position, player.position);

        switch (currentState)
        {
            case State.Patrol:
                Patrol();
                if (distance < chaseRange)
                    currentState = State.Chase;
                break;

            case State.Chase:
                Chase();
                if (distance > chaseRange * 1.5f)
                    currentState = State.Patrol;
                break;
        }
    }

    void Patrol()
    {
        transform.position = Vector2.MoveTowards(transform.position, patrolTarget, patrolSpeed * Time.deltaTime);
        if (Vector2.Distance(transform.position, patrolTarget) < 0.2f)
            ChooseNewPatrolPoint();
    }

    void Chase()
    {
        transform.position = Vector2.MoveTowards(transform.position, player.position, speed * Time.deltaTime);
    }

    void ChooseNewPatrolPoint()
    {
        patrolTarget = new Vector2(Random.Range(-8f, 8f), Random.Range(-4f, 4f));
    }
}

```

## OUTPUT:
<img width="400" height="250" alt="Screen Shot 1947-08-02 at 12 21 39" src="https://github.com/user-attachments/assets/6213fc90-740d-43c5-96eb-b200feccbc0a" />
<img width="400" height="250" alt="Screen Shot 1947-08-02 at 12 22 16" src="https://github.com/user-attachments/assets/d2093518-929f-4ae6-bb42-d1a16c1b7080" />

## RESULT:
Thus, the 2D Chase Escape game was successfully developed using Unity and adopted AI-based path-following and finite state machine (FSM) technology to simulate intelligent enemy behavior.

