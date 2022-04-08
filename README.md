using UnityEngine;
using UnityEngine.SceneManagement;

public class Control : MonoBehaviour {

    public GameObject cam;

    Quaternion StartingRotation;

    float Ver, Hor, Jump, RotMor, RotVer;

    bool isGround;

    public float Speed = 10, JumpSpeed = 200, senssensitivity = 5;

    private void Start()
    {
        StartingRotation = transform.rotation;
    }

    private void OnCollisionStay(Collision other)
    {
        if (other.gameObject.tag == "ground")
        {
            isGround = true;
        }
    }
    private void OnCollisionExit(Collision other)
    {
        if (other.gameObject.tag == "ground")
        {
            isGround = false;
        }
    }

    void FixedUpdate()
    {
        RotMor += Input.GetAxis("Mouse X") * senssensitivity;
        RotVer += Input.GetAxis("Mouse Y") * senssensitivity;

        RotVer = Mathf.Clamp(RotVer, -60, 60);

        Quaternion RotY = Quaternion.AngleAxis(RotMor, Vector3.up);
        Quaternion RotX = Quaternion.AngleAxis(-RotVer, Vector3.right);

        cam.transform.rotation = StartingRotation * transform.rotation * RotX;
        transform.rotation = StartingRotation * RotY;

        if (isGround)
        {
            Ver = Input.GetAxis("Vertical") * Time.deltaTime * Speed;
            Hor = Input.GetAxis("Horizontal") * Time.deltaTime * Speed;
            Jump = Input.GetAxis("Jump") * Time.deltaTime * JumpSpeed;

            GetComponent<Rigidbody>().AddForce(transform.up * Jump, ForceMode.Impulse);
        }
        transform.Translate(new Vector3(Hor, 0, Ver));
    }
}
