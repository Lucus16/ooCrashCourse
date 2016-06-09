# Object Orientation crash course

---
## Java types
- Builtin types: `byte`, `short`, `int`, `long`, `float`, `double`, `boolean`,
`char`
- Classes: Object, String, StringBuilder, Comparator<T>, etc...
- Interfaces: Observer, Comparable<T>, etc...
- An array type T[] for every type T, including array types themselves
- int[][] is just an array of int[]

---
## Fields
```java
class Pair {
	Object left;
	Object right;
}
```

---
## Constructor
```java
class Pair {
	Object left;
	Object right;

	Pair(Object left, Object right) {
		this.left = left;
		this.right = right;
	}
}
```

---
## Methods
```java
class Pair {
	Object left;
	Object right;

	Pair(Object left, Object right) {
		this.left = left;
		this.right = right;
	}

	Object getLeft() {
		return left;
	}

	Object getRight() {
		return right;
	}
}
```

---
## Static keyword
```java
class Pair {
	static int pairCount = 0;
	Object left;
	Object right;

	Pair(Object left, Object right) {
		this.left = left;
		this.right = right;
		pairCount++;
	}

	Object getLeft() { return left; }
	Object getRight() { return right; }

	static int getCount() {
		return pairCount;
	}
}
```

---
## Final keyword
```java
class Pair {
	final Object left;
	final Object right;

	Pair(Object left, Object right) {
		this.left = left;
		this.right = right;
	}

	Object getLeft() { return left; }
	Object getRight() { return right; }
}
```

---
## Public and private keywords
```java
public class Pair {
	private Object left;
	private Object right;

	public Pair(Object left, Object right) {
		this.left = left;
		this.right = right;
	}

	public Object getLeft() { return left; }
	public Object getRight() { return right; }
}
```

---
## Equals and hashCode
```java
class Pair {
	Object left;
	Object right;
	
	...

	boolean equals(Object obj) {
		if (!(obj instanceof Pair)) {
			return false;
		}
		Pair other = (Pair)obj;
		return this.left.equals(other.left) &&
				this.right.equals(other.right);
	}

	int hashCode() {
		int leftHash = (left.hashCode() << 16) + 
				(left.hashCode() >> 16);
		return leftHash + right.hashCode();
	}
}
```

---
## toString
```java
class Pair {
	Object left;
	Object right;

	...

	String toString() {
		return "(" + left.toString() + ", " +
				right.toString() + ")";
	}

	// Pretend that java has a useful
	// toString(StringBuilder sb) defined for Object
	void toString(StringBuilder sb) {
		sb.append("(");
		left.toString(sb);
		sb.append(", ");
		right.toString(sb);
		sb.append(")");
	}
}
```

---
## toString
```java
class Pair {
	Object left;
	Object right;

	...

	static String arrayToString(Pair[] pairs) {
		StringBuilder sb = new StringBuilder("[");
		boolean first = true;
		for (Pair pair : pairs) {
			sb.append(pair.toString());
			if (!first) { sb.append(", "); }
			first = false;
		}
		return sb.toString();
	}
}
```
Use StringBuilder whenever there is iteration or recursion.

---
## Inheritance
```java
class Pair extends Observable {
	Object left;
	Object right;

	Pair(Object left, Object right) {
		this.left = left;
		this.right = right;
	}

	Object getLeft() { return left; }
	Object getRight() { return right; }

	Object setBoth(Object left, Object right) {
		this.left = left;
		this.right = right;
		setChanged();
		notifyObservers();
	}
}
```

---
## Generics
```java
class Pair<L, R> {
	L left;
	R right;

	Pair(L left, R right) {
		this.left = left;
		this.right = right;
	}

	L getLeft() { return left; }
	R getRight() { return right; }

	public <N> Pair<N, R> newLeft(N left) {
		return new Pair<N, R>(left, right);
	}
}
```
The pattern for method declaration is:

`keywords` `<type parameters>` `return type` `method name` `(arguments)`
`{ body }`

---
## Generics and inheritance
```java

class Pair<L extends Comparable<L>, R extends Comparable<R>>
		extends Comparable<Pair<L, R>>{
	L left;
	R right;

	Pair(L left, R right) {
		this.left = left;
		this.right = right;
		pairCount++;
	}

	L getLeft() { return left; }
	R getRight() { return right; }

	int compareTo(Pair<L, R> other) {
		int left = this.left.compareTo(other.left);
		if (left != 0) { return left; }
		return this.right.compareTo(other.right);
	}
}
```

---
## On implementing interfaces
Make use of the other implementations! This works for equals, compareTo,
toString, hashCode, etc...

Builtin types have these as well, they look like this:

```java
Integer.equals(int1, int2);
Long.compare(long1, long2);
Double.hashCode(double1);
Character.toString(char1);
```

---
## Arrays vs ArrayLists
An `array` is not an `Object`, despite being constructed with the `new` keyword.
An `array` has a constant length, it can never be changed, HOWEVER:
A variable holding an `array` can point to arrays of different lengths at
different times.

```java
ArrayList<Integer> ints = new ArrayList<Integer>();
```

An `ArrayList` is an `Object`, so you can call methods on it. They need a type
parameter, it should never be omitted. There are wrappers for the primitive
types

| primitive | wrapper Object |
| --------- | -------------- |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| boolean | Boolean |

---
## ArrayLists vs LinkedLists
An `ArrayList` has fast access, but very slow insert and delete except at the end.
This is because access is by address and insertion and deletion means moving
everything that comes after it.

A `LinkedList` has slow access except at the start, but fast insert and delete.
This is because accessing means traversing all the links, while inserting and
deleting means only updating the previous and next links.
