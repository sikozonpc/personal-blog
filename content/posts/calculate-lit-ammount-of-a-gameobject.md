---
title: "Technique to calculate how much lit a Game Object is"
date: 2020-08-22
draft: false
description: "A way to calculate how much of an object surface is lit by a light in Unity 3D"
tags: [csharp, game-dev, computer-science]
---

Imagine if you're making a game where **the player can place farms and these need light in order to grow**.
There are a couple of ways to implement this behaviour and with this post I'll share my approach and some ideas on it.

### Design decisions

##### Solution #1

Firstly consider a very simple solution, where we just calculate the distance between the **Light Emittor** and the **Light Receiver**.

![Distance example](https://res.cloudinary.com/ddfkf8qha/image/upload/v1598118354/floating%20point%20images/distance-example_ihi67z.png)

```cs
var d = Vector3.Distance(lightReceiver.position, lightEmittor.position);

// Check if the receiver is too far away
lightReceiver.HasEnoughLight(d);
```

This would work, **however this solution doesn't care if the light is blocked by another entity in the middle**.

##### Solution #2

If you want to take into consideration any object in the middle, then let's go with a slower approach (it actually is not that slow if you start optimizing it).

Let's start by creating two behaviours/classes, the `LightEmittor` and the `LightReceiver`, their relation is one to many, 1 Emittor to many Receivers.

The idea is, the `LightEmittor` will **raycast** to the receiver and if the ray **hit data** reaches the entity, then it's not shadowed and is lit.

Ofcourse, you can send only one ray to the receiver body center and call the calculation done, but if you want a more accurate calculation you need to increase the amount of rays sent from the emittor.

These rays would solve the light collision issue and the distance from the light source issue (if the rays length are finite).

![Raycast solution](https://res.cloudinary.com/ddfkf8qha/image/upload/v1598119303/floating%20point%20images/raycast_emqy5q.png)

Now, we that we have a solution for the problem, we can start building it, but firstly let's address an issue: *What are the points the raycast should hit in order for an object to be lit ?*

To answer that, it's up to the game needs, but for simplicity I'll say it's when the 50% of the verticies of the LightReceiver are hit.

##### Implementation details

The implementation is quite simple, let's start by creating a Prefab and attach a Unity 3d Light component on it for the `LightEmittor` and attach a script.

```cs
using System.Collections.Generic;
using UnityEngine;

namespace Gameplay {
    [RequireComponent(typeof(Light))]
    public class LightEmittor : MonoBehaviour {

        // Debbuging purposes
        [SerializeField]
        List<LightReceiver> lightReceivers = new List<LightReceiver>();
        float range;

        void Start() {
            range = GetComponent<Light>().range;
            FindLightReceiversInRange();
        }

        void Update() {
            SendRaysToLightReceivers();
        }

        void LateUpdate() {
            FindLightReceiversInRange();
        }

        void OnDrawGizmosSelected() {
            // Debug rays in the editor when playing
            Gizmos.DrawWireSphere(transform.position, GetComponent<Light>().range);
        }

        void FindLightReceiversInRange() {
            // Get all objects in range with the LIGHT_RECEIVER layer mask
            Collider[] colliders = Physics.OverlapSphere(transform.position, range, LayerMask.GetMask("LIGHT_RECEIVER"));

            foreach (var collider in colliders) {
                LightReceiver lightReceiverObject;
                collider.TryGetComponent(out lightReceiverObject);

                if (lightReceiverObject != null && !lightReceivers.Contains(lightReceiverObject)) {
                    lightReceivers.Add(lightReceiverObject);
                }
            }
        }

        void SendRaysToLightReceivers() {
            foreach (LightReceiver receiver in lightReceivers) {
                Matrix4x4 receiverMatrix = receiver.transform.localToWorldMatrix;

                //TODO: Performance improvement: maybe the LightReceiver can expose directly its coords to ray cast on
                MeshFilter receiverMeshFilter = receiver.GetComponent<MeshFilter>();

                float vertsLit = 0;
                foreach (var vert in receiverMeshFilter.mesh.vertices) {

                    Vector3 world_v = receiverMatrix.MultiplyPoint3x4(vert);
                    Vector3 dir = (world_v - transform.position).normalized;

                    RaycastHit hit;
                    if (Physics.Raycast(transform.position, dir, out hit, range, LayerMask.GetMask("LIGHT_RECEIVER", "BUILDING"))) {

                        bool hitAReceiver = hit.collider.CompareTag("LIGHT_RECEIVER");
                        if (hitAReceiver) {
                            Debug.DrawRay(transform.position, dir * range, Color.blue);
                            vertsLit++;
                        } else {
                            Debug.DrawRay(transform.position, dir * range, Color.red);
                        }
                    }
                }

                float allVerts = (float)receiverMeshFilter.mesh.vertexCount;
                receiver.LitVertexPercentage = (vertsLit / allVerts) * 100;
            }
        }
    }
}
```

Notes: 
- The `FindLightReceivers()` uses LIGHT_RECEIVER layer mask to filter close by receivers, it's up to you how you find them.

The meat of this class is the `SendRaysToLightReceivers()` where it does what I designed before, where `BUILDING` are player placed buildings that might be shadowing a farm.

And for the `LightReceiver`:

```cs
using UnityEngine;

namespace Gameplay {
    public class LightReceiver : MonoBehaviour {
        public float LitVertexPercentage = 0f;
    }
}
```

All it needs is a Prefab object with a Unity `MeshCollider`.

Now here, very important, the LightReceiver prefab **I used has a simple cube with 8 verticies** with the `MeshRenderer` disabled so it doesnt have a visual mesh in game where I just attached it to Farm game object has a child, the reason for this is **so it only calculates the 8 verticies instead of the thousands and unexpected verticies a Farm mesh might have**, this way we optimise a lot in the calculation aswell we **decouple the behaviour from the Farm mesh iteself**.

![Prefab](https://res.cloudinary.com/ddfkf8qha/image/upload/v1598120404/floating%20point%20images/prefab_qqhpwe.png)

![Sample](https://res.cloudinary.com/ddfkf8qha/image/upload/v1598120715/Sem_T%C3%ADtulo_qnf0ob.png)

This solution has a lot of potential for this problem but if you're thinking of implementing it in a real world scenario you have a lot of freedom on how you send the raycast and how many, or even where you place the verticies.

Hope my solution helps and happy coding! 