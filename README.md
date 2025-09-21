<h1>
Unity Collider Support (Deprecated)
</h1>

<h2>
Deprecation Reason
</h2>

A Collision stores the contacts.
Contacts are the "collidier - collider" collision data such as "Where does this collider contacts with other?".

A Collision starts to occur when Collider component(s) of a dynamic Rigidbody collides with other Rigidbody's Collider component(s) or a Static Collider.
After collision started, the Collision stores the contacts.
<b> So the Collision is belong to Rigidbody and not the Collider. </b>

Imagine a hierarchy like this:
<ul>
	<li>
		Rigidbody A
	<ul>
		<li>
			Collider A, Collider B
		</li>
	</ul>
	</li>
	<li>
		Rigidbody B
	<ul>
		<li>
			Collider C, Collider D
		</li>
	</ul>
</ul>

<b> OnCollisionEnter</b>: If any number of the Collider components of Rigidbody A, collides with any number of Collider components of Rigidbody B, then a Collision occurs. Gets called only when a Collision starts.

<b> OnCollisionStay</b>: If any number of the Collider components of Rigidbody A are still colliding with Rigidbody B, then collision continues. Gets called when Collision continues.

<b> OnCollisionExit</b>: If none of the Collider component of Rigidbody A colliding with Rigidbody B, then collision finishes. Gets called when Collision finished.

So, the <b>OnCollisionEnter</b> and <b>OnCollisionExit</b> methods are called once when the first physical contact occurs, and last contact is out between two Rigidbody or one Rigidbody and Static Collider.

For the OnTriggerXXX methods, they are already good. You SHOULDNT rely on caching which component are inside. Instead, use Components. Enable/Disable them.
Or use contact data, if that is what you want.

<h2>
Documentation
</h2>

Made in Unity 6000.0.13f1 and didnt tested in other environments.

By design (hate that word), Unity **only** sends collision messages to the hierarchy of Rigidbody. And also Unity does not sends `OnTriggerExit()` message to self whenever other collider is disabled or destroyed. This package aims to solve that problem.

I did my best to not hit the performance while supporting anything i thought of. So i say, package creates zero garbage but iterates through dictionaries by twice. Your help may make this package faster by sending a pull request.

<h3>
ChildCollisionNotifier
</h3>


To send collision messages to children, put this to Rigidbody hierarchy and it will notify to listeners. This is an example hierarchy:
+ Rigidbody with ChildCollisionNotifier
  + Any MonoBehaviour ICollisionEnterListener
  + Any MonoBehaviour ICollisionStayListener
  + Any MonoBehaviour ICollisionExitListener
  + Any MonoBehaviour ICollisionExitDisabledListener - **Other** collider must have DisabledColliderNotifier at its hierarchy.

<h3>
DisabledColliderListenerTrigger
</h3>

Allows self game object to take ITriggerExitDisabledListener message by listening to DisabledColliderNotifier(s). You can think that component as a trigger. If it is not there, it wont get notified by DisabledColliderNotifier.

<h3>
DisabledColliderNotifier
</h3>

Notifies whenever a collider gets disabled. As a downside, destroying a collider in the hierarchy is not allowed and notifier wont send message. Only sends whenever collider's GameObject gets destroyed.

<h2>
TODO
</h2>

<ol>
	<li>
		DisabledColliderNotifier can detect destroyed colliders before it gets destroyed
	</li>
</ol>