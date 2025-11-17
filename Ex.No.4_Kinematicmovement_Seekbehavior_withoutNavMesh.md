# Ex.No: 4  Implementation of Kinematic movement -seek and Flee behavior in Unity
### DATE: 26-08-2025                                                                       
### REGISTER NUMBER : 212222110051
### AIM: 
To write a program to simulate the process of seek and Flee behavior in Unity without NavigationMeshAgent. 
### Algorithm:
1. Create a New Unity Project by Open the  Unity Hub and create a new 3D Project,Name the project (e.g., SeekBehaviorDemo).
2. Create the Moving Object
   In the Hierarchy, right-click → 3D Object → Cube (or Sphere).
   Rename it to Seeker and Reset its position:Select the Seeker, go to Inspector → Transform → Set Position to (0,0,0).
3. Create the Target Object
   Right-click in the Hierarchy → 3D Object → Sphere (or any other shape).
   Rename it to Target. Move it away from Seeker, e.g., set Position to (5, 0, 5).
   Select the Target, add a Material, and change the color. (if needed) 
4. Adding the Seek Behavior Script
   Create the Script-In the Project Window, go to the Assets folder.
   Right-click → Create → C# Script.
5. Write a script for seek behavior and save it
6. Attach the Script
   Select Seeker in the Hierarchy - Drag & Drop the SeekBehavior script onto the Inspector Panel.
   Drag & Drop the Target from the Hierarchy into the "Target" field in the script component.
12.  Write a script for flee behavior and attach it to target
13.  Run the game
14. Stop the program
    
### Program:
Seek
```
using UnityEngine;
public class Script1 : MonoBehaviour
{
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    public Transform target;
    public float speed;
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        Vector3 direction = (target.position - transform.position).normalized;
        transform.position += direction * speed * Time.deltaTime;   
    }
}


```

Flee
```
using UnityEngine;

public class Script2 : MonoBehaviour
{
    public Transform target;   
    public float speed = 5f;   
    void Start()
    {

    }
    
    void Update()
    {
        if (target == null) return;  
        Vector3 direction = (transform.position - target.position).normalized;
        transform.position += direction * speed * Time.deltaTime;
    }
}

```
### Output:
Seek and Flee
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/22049db6-f52c-4be1-b74f-ef63e1f0dca9" />


### Result:
Thus the simple seek behavior was implemented successfully.
