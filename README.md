# How to check if an element exists in a list

### 1. HashMap / HashSet / HashTable

A HashMap is a map used to store mappings of key-value pairs.
It is a data structure which allows us to store object and retrieve it in constant time O(1) provided we know the key.

HashMap contains an array of the nodes, and the node is represented as a class. 
It uses an array and LinkedList data structure internally for storing Key and Value.

![img](imgs/hashmap.png)

Before understanding the internal working of HashMap, you must be aware of hashCode() and equals() method.

* **equals()**: It checks the equality of two objects, whether they are equal or not. It is a method of the Object class which can be overridden.
  If you override the equals() method, then it is mandatory to override the hashCode() method.
* **hashCode()**: The **INTEGER** value received from this method is used as the bucket number. The bucket number is the address of the element inside the map. 
  Hash code of null Key is 0.
* **Buckets**: The array of the node is called buckets. Each node has a data structure like a LinkedList. More than one node can share the same bucket.


![img](imgs/hashmap_working.png)

##### What put() Method Does

* Step 1. The key object is checked for null, and if true the value is stored in  table[0] position, because hashcode for null is always 0.
* Step 2. A hash value is calculated using the key’s hash code by calling its ***hashCode()*** method. 
This hash value is used to calculate the index in the array for storing the Entry objects. 
```
NOTE: JDK designers well assumed that there might be some poorly written  hashCode() functions that can return very high or low hash code value.<br> 
To solve this issue, they introduced another hash() function and passed the object’s hash code to this hash() function to bring hash value in the range of array index size.

```

* Step 3.
Now the indexFor(hash, table.length) the function is called to calculate the exact index position for storing the Entry object.
  
```index = hash(key) & (n-1)``` 

##### How Collisions Are Resolved

Now, as we know that two unequal objects can have the same hash code value, these two different objects will be stored in the same array location called a bucket.
The Entry class has an attribute "next." This attribute always points to the next object in the chain. This is exactly the behavior of the LinkedList.

So, in case of collision, Entry objects are stored in a linked list form. 
When an Entry object needs to be stored in a particular index, HashMap checks whether there is already an entry. 
* If there is no entry already present, the entry object is stored in this location.
* If there is already an object sitting on a calculated index, its next attribute is checked. 
  * If it is null, and the current entry object becomes the next node in LinkedList. 
  * If the next variable is not null, the procedure is followed until the next is evaluated as null.

###### What if we add another value object with the same key as entered before? 
After determining the index position of the Entry object, while iterating over LinkedList on the calculated index, HashMap calls equals method on the key object for each entry object. <br>
All these entry objects in LinkedList will have similar **hashcode()** but **equals()**  method will test for true equality. <br>
If key.equals(k) will be true then both keys are treated as the same key object. This will cause the replacement of value objects inside the entry object only.



As you can see, for storing N objects we would need an object storage of size N, with ~O(1) lookup.
However, as the size of the list increases, we might need a different approach.

### 2. BloomFilter

