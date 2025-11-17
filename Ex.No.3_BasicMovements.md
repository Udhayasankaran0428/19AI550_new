# Ex.No: 3  Basic movements in Unity 
### DATE:  18/08/25                                                                          
### REGISTER NUMBER : 212222110051
### AIM: 
 To learn the basic movements translation,scaling and rotation of game objects through code.
### Procedure:
1. Setup the Scene
2. Open Unity and create a 3D Scene.
3. Add three objects:Cube → Rename to Object1 (for movement),Sphere → Rename to Object2 (for rotation).Capsule → Rename to Object3 (for scaling).
4. Add the Script,Create a C# Script → Name it TransformOperations.cs.
5. Write the code for translation,scaling and rotation,save and close the script
6. Save the script
7. Select any empty GameObject (or create one: GameObject → Create Empty).
8. Attach the TransformOperations script to it.
9. In the Inspector, assign Object1 → Drag the Cube,Object2 → Drag the Sphere.Object3 → Drag the Capsule.
10. Run the Scene Press Play ▶️ in Unity
11. Stop the program.
### Program 
```
using UnityEngine;

public class BallMovement : MonoBehaviour
{
    public float moveSpeed = 5f; // Movement speed
    public float jumpForce = 5f; // Jump strength
    private Rigidbody rb;

    void Start()
    {
        // Get the Rigidbody component
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        // Get player input (WASD or Arrow Keys)
        float moveX = Input.GetAxis("Horizontal");
        float moveZ = Input.GetAxis("Vertical");

        // Apply movement
        Vector3 movement = new Vector3(moveX, 0f, moveZ) * moveSpeed;
        Vector3 newVelocity = new Vector3(movement.x, rb.linearVelocity.y, movement.z);
        rb.linearVelocity = newVelocity;

        // Jump (optional)
        if (Input.GetKeyDown(KeyCode.Space) && IsGrounded())
        {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
        }
    }

    // Check if the ball is touching the ground
    bool IsGrounded()
    {
        return Physics.Raycast(transform.position, Vector3.down, 0.6f);
    }
}

```
### Output:
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/7637b53b-023e-49c5-abf7-5c547c786635" />

### Result:
Thus the basic movement is learned through scripting


