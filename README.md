# 3D_Tank

1.Map

![image](https://user-images.githubusercontent.com/48191157/74037614-f55a9b00-4a01-11ea-9323-42ac5045cd7f.png)

2.랜덤으로 떨어지는 미사일

![image](https://user-images.githubusercontent.com/48191157/74038023-d01a5c80-4a02-11ea-9a7c-ac2d1e6be308.png)

    void OnCollisionEnter(Collision other)
  	{
		Instantiate (prefab, transform.position, Quaternion.identity);

		if (!other.gameObject.CompareTag ("terrain")) {
			//자식들에 대해서 부모를 null로 만들어줌
			Transform[] children = other.gameObject.GetComponentsInChildren<Transform> ();
			for (int i = 0; i < children.Length; ++i) {
				children [i].SetParent (null);
				Rigidbody rbody = children [i].GetComponent<Rigidbody> ();
				Collider col = children [i].GetComponent<Collider> ();
				if ((children [i].GetComponent<Renderer> () == null && col != null))
					col.enabled = false;
				else {
					if (children [i].GetComponent<Collider> () == null)
						children [i].gameObject.AddComponent<BoxCollider> ();
					if (rbody == null)
						rbody = children [i].gameObject.AddComponent<Rigidbody> ();
					rbody.AddExplosionForce (explosionForce, transform.position, explosionRadius, 0f, ForceMode.Force);
					}
				}
			}
		Destroy (ms);
	}

3.AI탱크와 격돌

![image](https://user-images.githubusercontent.com/48191157/74038104-f63ffc80-4a02-11ea-82a3-fa2466b24497.png)

	void OnTriggerEnter(Collider other){
		if (other.tag == "WAY_POINT") {
			nextIdx = ((++nextIdx) >= points.Length) ? 1 : nextIdx
      }
	}           //AI이동

    void Update () {
		Vector3 fwd = transform.TransformDirection (Vector3.down);

		if (!(this.transform.parent == null)) {
			float dist = Vector3.Distance (this.transform.position, Player.position);

			if (dist <= 38) {           //거리가 가까워지면 공격
				this.transform.LookAt (Player.transform);
				this.transform.Rotate (-90.0f, 0.0f, 0.0f);
				if (fire) {
					GameObject ballet = Instantiate (ball, ballPoint.transform.position, Quaternion.identity);
					ballet.GetComponent<Rigidbody> ().AddForce (fwd * power * 10);
					StartCoroutine (FireReload ());
					time = 2.0f;
					firingAudio.Play ();
				}
			} else {
				transform.rotation = turret.rotation;
			}
			if (time > 0)
				time--;
			if (time == 0f)
				lightEff.intensity = 0f;
		}
	}

4.점령전

![image](https://user-images.githubusercontent.com/48191157/74038117-fcce7400-4a02-11ea-9c34-cfd3826af0a4.png)

