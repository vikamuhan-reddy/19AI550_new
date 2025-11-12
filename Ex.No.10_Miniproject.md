# Ex.No: 10  Implementation of 2D/3D Game
### DATE: 24/10/25
### REGISTER NUMBER: 212223240181

---

## AIM:
To develop a **flappy bird* in Unity using an **AI-based enemy** that intelligently follows the player.

---

## ALGORITHM:
1. Start Unity and create a new 2D project named **“FlappyBirdAI”**.
2. Design the game scene by adding **Bird** (player sprite), **Pipes**, **Ground**, and **AI Enemy Bird** (red bird sprite).
3. Assign **BoxCollider2D** and **Rigidbody2D** components to Bird, AI Enemy Bird, Pipes, and Ground.
4. Attach a **BirdScript** to control flapping using **Space key**.
5. Attach a **PipeSpawnScript** to spawn moving pipes at random heights.
6. Attach a **PipeMoveScript** to make pipes scroll left and destroy when off-screen.
7. Attach a **PipeMiddleScript** to detect when the bird passes a pipe and increase score.
8. Attach an **EnemyBirdScript** to make the AI enemy bird intelligently follow and predict the player’s vertical position.
9. Implement **collision detection** in BirdScript and EnemyBirdScript to trigger *Game Over* when hitting pipes, ground, or enemy.
10. Create a **LogicScript** with UI to display score and show *Game Over* screen.
11. Test the game by running and adjusting flap strength, pipe speed, and AI prediction distance.
12. Verify that pipes spawn correctly, score increases, AI enemy dynamically chases the player’s height, and collision ends the game.

---

## PROGRAM:

### **BirdScript.cs**
```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class BirdScript : MonoBehaviour
{
    public Rigidbody2D myRigidbody;
    public float flapStrength;
    public LogicScript logic;
    public bool birdIsAlive = true;
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        logic = GameObject.FindGameObjectWithTag("Logic").GetComponent<LogicScript>();

    }

    // Update is called once per frame
    void Update()
    {
        if(Input.GetKeyDown(KeyCode.Space) && birdIsAlive)
        {
            myRigidbody.linearVelocity = Vector2.up * flapStrength;
        }
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        logic.gameOver();
        birdIsAlive = false;
    }
}

```

### **PipeSpawnScript.cs**
```csharp

using UnityEngine;

public class PipeSpawnScript : MonoBehaviour
{
    public GameObject pipe;
    public float spawnRate = 2;
    public float timer = 0;
    public float heightOffset = 10;
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        spawnPipe();
    }

    // Update is called once per frame
    void Update()
    {
        if(timer < spawnRate)
        {
            timer = timer + Time.deltaTime;
        }
        else
        {

            spawnPipe();            
            timer = 0;
        }
    }

    void spawnPipe()
    {
        float lowestPoint = transform.position.y - heightOffset;
        float highestPoint = transform.position.y + heightOffset;
        Instantiate(pipe,new Vector3(transform.position.x, Random.Range( lowestPoint, highestPoint), 0), transform.rotation);
    }
}


```

### **LogicScript.cs**

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class LogicScript : MonoBehaviour
{
    public int playerScore;
    public Text scoreText;
    public GameObject gameOverScreen;

    [ContextMenu("Increase Score")]
    public void addScore(int scoreToAdd)
    {
        playerScore = playerScore + scoreToAdd;
        scoreText.text = playerScore.ToString();
    }

    public void restartGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }

    public void gameOver()
    {
        gameOverScreen.SetActive(true);
    }
}

```

### **PipeMiddleScript.cs**

```csharp
using UnityEngine;

public class PipeMiddleScript : MonoBehaviour
{
    public LogicScript logic;
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        logic = GameObject.FindGameObjectWithTag("Logic").GetComponent<LogicScript>();
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.gameObject.layer == 3)
        {
            logic.addScore(1);
        }
    }
}

```

### **PipeMoveScript.cs**
```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class PipeMoveScript : MonoBehaviour
{
    public float moveSpeed = 5;
    public float deadZone = -45;
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        transform.position = transform.position + (Vector3.left * moveSpeed) * Time.deltaTime;

        if(transform.position.x < deadZone)
        {
            Debug.Log("Pipe Deleted");
            Destroy(gameObject);
        }
    }
}

```
## OUTPUT:
<img width="890" height="495" alt="Screenshot 2025-11-03 192447" src="https://github.com/user-attachments/assets/e8ece551-638c-4a55-bc94-bfe48c63f67b" />
<img width="845" height="480" alt="Screenshot 2025-11-03 192456" src="https://github.com/user-attachments/assets/cd928fe5-55ef-4473-b42d-aa0f53631849" />


## RESULT:
Thus, flappy bird game is done successfully.

