Mike's Mini Golf
================

![thumbnail](https://user-images.githubusercontent.com/61642101/230894357-90451e62-71a0-42d2-867f-b62c9cb67599.png)

Welcome to my mini golf game! This is a simple and fun project I worked on.<br> It was made in Unity and was inspired by my friend who really likes to play golf.

Instructions - How to play
------------------

- Hold W key to gain power. Ball is hit when you let go of W key.
- Left and right arrow keys turn the camera view.
- Try not to fall into the void.
- Try to get into the hole (theres no award)

[CLICK HERE TO PLAY](mikes_mini_golf/index.html)

Code 
------
Hey it's private!
<br>But I'll give you a bit of info.
<br>Shhhhhh... Don't tell Mike I'm doing this.
<br>So you gain power by holding down then key W
<br>And theres a ball object that has a RigidBody that gets force added when you let go of the key W.
<br>The power of the force is determined by how long you hold down W,
<br>The red bar on the side also changes based on how much power you have gained.


### Hit Script

  
  
~~~c
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;
using UnityEngine.UI;

public class hit : MonoBehaviour
{
    public Image powerBar;
    public GameObject ball;
    float power = 5f;
    Vector3 startingOffset;
    void Start()
    {
        startingOffset = transform.localPosition;
    }

    void Update()
    {
        transform.localScale = new Vector3(0.05f,0.05f,power/10);
        transform.localPosition = new Vector3(0, 0, -(power-2)/20)+startingOffset;
        if (Input.GetKey("w"))
        {
            if (power < 10)
            {
                power += 5 * Time.deltaTime;
                powerBar.GetComponent<RectTransform>().sizeDelta= new Vector2(30f,(power - 2) * 41.1625f );
            }
        }
  
        if (Input.GetKeyUp("w"))
        {
            ball.GetComponent<Rigidbody>().AddForce(transform.forward * power, ForceMode.Impulse);
            power = 2;
            powerBar.GetComponent<RectTransform>().sizeDelta = new Vector2(30f, 0);
        }
    }
}
~~~

There's also a script that moves the club and camera so you can apply the force in the direction you want.

### Club Script
  
~~~c
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class club : MonoBehaviour
{

    public GameObject ball;
    void Update()
    {
        transform.position = ball.transform.position;

        // transform.localEulerAngles = ball.transform.localEulerAngles;
        
        if (Input.GetKey("left"))
        {
            transform.localEulerAngles += new Vector3(0, 120, 0)*Time.deltaTime;
        }
        if (Input.GetKey("right"))
        {
            transform.localEulerAngles -= new Vector3(0, 120, 0) * Time.deltaTime;
        }
    }
}

~~~
 
### Camera Follow Script

~~~c
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraFollow : MonoBehaviour
{
    public Transform target;
    public float smoothTime = 0.3f;
    private Vector3 velocity = Vector3.zero;
    public Vector3 offset;

    void Update()
    {
        // Define a target position above and be$$anonymous$$nd the target transform
        Vector3 targetPosition = target.transform.position+offset;
        // transform.position = Vector3.SmoothDamp(transform.position, targetPosition, ref velocity, smoothTime);

        transform.position = targetPosition;
        transform.LookAt(target);
        // transform.RotateAround(Vector3.right, 5f);
    }

}
~~~

Lastly, there's a script to detect when you have fallen off the map and gets you back to the start.

### Restart Script

~~~c
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class DeathByFallingOffTheMap : MonoBehaviour
{

    // Start is called before the first frame update
    void OnTriggerEnter(Collider collider)
    {
        if (collider.tag == "Player")
        {
            SceneManager.LoadScene(SceneManager.GetActiveScene().name);

            DeathCount.Instance.timesDied += 1;
        }
    }
}
~~~

And that's it for the basics of the game!

![win](https://user-images.githubusercontent.com/61642101/230899820-c8b60f06-0068-4b2f-a03b-2eb8f46dbb9e.png)

~~~diff
- Now would you please stop snooping around in the file and go play the game? -
~~~
