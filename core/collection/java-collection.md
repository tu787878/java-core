# 1. Wha is the difference beetween the List, Set, Map?

- ***List***: allows duplicate elements, maintains the insertion order.
- ***Set***: does not allow duplicate elements
- ***Map***: mapping of key value and does not allow duplicate keys

# 2. ArrayList vs LinkedList

- ArrayList: uses a dynamic array to store the elements.
> - fast random access
> - slower insertion and deletion
- LinkedList: uses a doubly-linked list to store elements.
> - fast insertion and deletion
> - slower random access

# 3. hashCode() and equals()

- HashMap uses hashCode() for the key. By default is the  of the object.
- A Set is a collection that does not allow duplicate elements. It enforces uniqueness based on the equals() method. By default is the references of the object.

# 4. HashSet, TreeSet, and LinkedHashSet
- HashSet does not maintain any order.
- TreeSet mantains elements in sorted order.
- LinkedHashSet maintains in the order they were added.