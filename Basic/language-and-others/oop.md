# OOP

实现一个聊天室的逻辑，有Room和User两个类

Room设置有一个门，而且有一个“主人”的设定，只有主人才能开门和关门

请问开门和关门方法，你会放到Room类还是User类中实现，为什么？

```java
Room:
	open(user){
		if user in my_user:
			this.room.set_state(opened)
		}

User:
	open(room){
		if room in my_room:
			room.set_state(opened)
		}
```

我会把 `open/close` 放在 `Room` 中。因为门的状态是 Room 的内部状态，开关门行为本质是在修改 Room 的状态；同时权限校验（是否为 owner）也是 Room 的业务规则。这样可以保持封装性，让 Room 对自己的状态和规则负责，而 User 只作为操作发起者传入。调用形式为 `room.open(user)`，而不是 `user.open(room)`。
