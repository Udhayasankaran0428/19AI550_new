# Ex.No: 10  Implementation of 2D/3D game -------------------
### DATE:                                                                            
### REGISTER NUMBER : 
### AIM: 
To develop a game -------------------------in Unity 
### Algorithm:
```
Player.cs
1. Initialize SpriteRenderer component.
2. Start sprite animation loop every 0.15 seconds.
3. On enabling the player:
      a. Reset vertical position to 0.
      b. Reset movement direction to zero.
4. Every frame (Update):
      a. If space key or left mouse button pressed, set upward movement direction.
      b. Apply gravity to the vertical direction.
      c. Update player position based on direction.
      d. Tilt the player based on vertical movement speed.
5. Animate player sprite by cycling through sprite frames.
6. On collision with:
      a. Obstacle tag → trigger game over.
      b. Scoring tag → increase score.
```
```
GameManager.cs
1. On awake:
    a. Ensure singleton instance of the game manager.
2. On start:
    a. Pause the game (stop time, disable player controls).
3. Pause() method:
    a. Set game time scale to 0 (freeze game).
    b. Disable player controls.
4. Play() method:
    a. Reset score to zero and update UI.
    b. Hide play button and game over UI.
    c. Enable player controls and start game (set time scale to 1).
    d. Find all existing pipes and destroy them to reset obstacles.
5. GameOver() method:
    a. Show play button and game over UI.
    b. Pause the game.
6. IncreaseScore() method:
      a. Increment score and update UI.
```
```
HurdleSpawner.cs
1.Initialize required variables such as hurdle prefab, spawn rate, Y-range, and movement speed.
2.Start the hurdle spawning coroutine when the game begins.
3.Inside the coroutine loop:
    a. Wait for the given spawnRate duration.
    b. Instantiate a new hurdle at the spawner’s current position.
    c. Generate a random Y-position between minY and maxY and apply it to the hurdle.
4.Assign leftward movement to the hurdle using Rigidbody2D linear velocity.
5.Destroy the spawned hurdle after 10 seconds to avoid unnecessary buildup.
6.Continue the loop indefinitely to keep spawning hurdles.

```
### Program:
```
player.cs
using UnityEngine;
using UnityEngine.UI;

public class PlayerControl : MonoBehaviour
{
    [Header("Movement")]
    public float moveSpeed = 5f;
    public float jumpForce = 10f;
    private Rigidbody2D rb;

    [Header("Ground Check")]
    private bool isGrounded = true;
    
    [Header("Game Management")]
    public GameManager gameManager; 

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        
        // Find the GameManager in the scene if we forgot to link it
        if (gameManager == null)
        {
            // --- FIXED WARNING ---
            gameManager = Object.FindAnyObjectByType<GameManager>();
        }
    }

    void Update()
    {
        // Get input from A/D keys or Left/Right arrows (-1 for left, 1 for right)
        float moveInput = Input.GetAxisRaw("Horizontal");
        
        // Set the player's physics velocity
        // --- FIXED WARNING ---
        rb.linearVelocity = new Vector2(moveInput * moveSpeed, rb.linearVelocity.y);

        // --- Jumping (this is the same as before) ---
        if ((Input.GetKeyDown(KeyCode.Space) || Input.GetMouseButtonDown(0)) && isGrounded)
        {
            rb.AddForce(new Vector2(0, jumpForce), ForceMode2D.Impulse);
            isGrounded = false;
        }
    }

    // This function checks for SOLID collisions
    private void OnCollisionEnter2D(Collision2D collision)
    {
        // Check if the player landed on the "Ground"
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }

        // Check if the player ran into a "Hurdle"
        if (collision.gameObject.CompareTag("Hurdle"))
        {
            // Tell the GameManager that the game is over
            if (gameManager != null)
            {
                gameManager.GameOver();
            }
            
            // --- FIXED WARNING ---
            rb.linearVelocity = Vector2.zero; // Stop the player
            enabled = false; // Disable this script
        }
    }

    // This function checks for TRIGGER collisions (like coins)
    private void OnTriggerEnter2D(Collider2D other)
    {
        // Check if the object we hit has the "Coin" tag
        if(other.CompareTag("coin"))
        {
            // Tell the GameManager to add score
            if (gameManager != null)
            {
                gameManager.AddScore(1);
            }
            
            // Destroy the coin object
            Destroy(other.gameObject); 
        }  
    }
}
```
```
GameManager.cs
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement; 
using TMPro;

public class GameManager : MonoBehaviour
{
    public int score = 0;
    public TextMeshProUGUI scoreText; 
    public GameObject gameOverPanel; 

    public PlayerControl player; 

    void Start()
    {
        if (gameOverPanel != null)
        {
            gameOverPanel.SetActive(false);
        }
        
        Time.timeScale = 1;

        // Find the player if we forgot to link it
        if (player == null)
        {
            player = Object.FindAnyObjectByType<PlayerControl>();
        }

        // --- THIS IS THE NEW LINE ---
        // Set the score text to "SCORE: 0" when the game starts
        if (scoreText != null)
        {
            scoreText.text = "SCORE: " + score;
        }
        // -----------------------------
    }

    public void AddScore(int amount)
    {
        score += amount;
        
        if (scoreText != null)
        {
            scoreText.text = "SCORE: " + score;
        }
    }

    public void GameOver()
    {
        Time.timeScale = 0; 
        
        if (gameOverPanel != null)
        {
            gameOverPanel.SetActive(true);
        }

        if (player != null)
        {
            player.enabled = false;
        }
    }

    public void RestartGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }
}
```
```
HurdleSpawner

using System.Collections;
using UnityEngine;

public class HurdleSpawner : MonoBehaviour
{
    
    public GameObject hurdlePrefab;
    
    public float spawnRate = 2f;
    
    public float minY = -1f;
    public float maxY = 1f;


    public float moveSpeed = 5f;

    void Start()
    {
        
        StartCoroutine(SpawnHurdles());
    }

    private IEnumerator SpawnHurdles()
    {
        
        while (true)
        {
            
            yield return new WaitForSeconds(spawnRate);
            GameObject newHurdle = Instantiate(hurdlePrefab, transform.position, Quaternion.identity);
            float randomY = Random.Range(minY, maxY);
            newHurdle.transform.position += new Vector3(0, randomY, 0);
            newHurdle.GetComponent<Rigidbody2D>().linearVelocity = new Vector2(-moveSpeed, 0);
            
            Destroy(newHurdle, 10f);
        }
    }
}

```
### Output:
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/0f3fce40-617f-4144-8219-de210a8e078a" />
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/672777bb-6769-4811-b634-9ace4e4942f6" />

### Result:
Thus the game was developed using Unity and adopted _-----------AI technology.
