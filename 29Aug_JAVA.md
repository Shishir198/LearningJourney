Credits - https://www.youtube.com/watch?v=nEno48RpDR4

<h1>Streams in Java8</h1>

- The power of stream shines while using operations on bulk data as it can do parallel processing .

![image](https://github.com/user-attachments/assets/9631e203-fd4f-4e17-9174-68780cbf3954)

![image](https://github.com/user-attachments/assets/060a61af-811b-4de2-bf34-f1efbdc866f2)

- Intermediate operations like filter , map etc are lazy in nature i.e executed only when terminal operations like collect , count is triggered.

![image](https://github.com/user-attachments/assets/7c4c146e-2d93-4f29-aafb-638a317a8a96)

![image](https://github.com/user-attachments/assets/24e08357-ef2c-4aab-85d4-5a81c9bc4bf8)

![image](https://github.com/user-attachments/assets/3a46e85c-831e-4fb5-bfca-30c12eaebd5f)

- Iterate is just like for loop which has initialization , operations and increment .

![image](https://github.com/user-attachments/assets/c6f03304-4d51-446d-8c5f-fabba28a3ca8)

![image](https://github.com/user-attachments/assets/3939e7fd-e251-4dd6-9232-fe8db0b7d84a)

![image](https://github.com/user-attachments/assets/629aebfa-0519-4901-ac51-0d5c750b82da)

![image](https://github.com/user-attachments/assets/70a99600-7bc6-4f3e-9ab4-99592025d77b)

- FLatmap is used in complex datastructures which is used to flatten out all the values in single stream .
-  Lets consider a List of Lists
-  ![image](https://github.com/user-attachments/assets/fd224ff0-188f-42c7-b11e-26bbf3c11a03)
-  Flatmap will convert this 2d ds into 1d stream , by collecting all the data and aligning it into linear fashion
-  According to image , [1,2,3] is in another list , [4,5,6] is in another . After flatmap - [1,2,3,4,5,6] .

![image](https://github.com/user-attachments/assets/72eb8ec7-5703-481a-a03c-e4a8e16e769e)


![image](https://github.com/user-attachments/assets/d915df84-d726-4859-b123-5bc6b1661192)

- with custom comparator

![image](https://github.com/user-attachments/assets/a127ab69-6750-4caa-abb2-3b6ab9d81a13)


![image](https://github.com/user-attachments/assets/5fb5b0d3-ac64-4faa-a14d-9e162aeac7d0)


![image](https://github.com/user-attachments/assets/159cef7f-c17a-4b16-b66d-5769aed0bc17)


![image](https://github.com/user-attachments/assets/d49f0c2d-8012-44d2-aaf6-5bd388d3484a)


![image](https://github.com/user-attachments/assets/43d6192a-c5aa-464f-8e5e-ad98bd4b7fef)


![image](https://github.com/user-attachments/assets/9f570268-483a-4b27-87b3-61867d42ed88)


![image](https://github.com/user-attachments/assets/99ae0ddb-a955-4b13-8fb4-8b7e8d0097fe)


![image](https://github.com/user-attachments/assets/3e798643-fb0f-43cb-a625-12328d37bc40)


![image](https://github.com/user-attachments/assets/7bc9e788-459c-41ba-8465-5ff2d99e6e6f)

- Because , this is how the sequence of operatrions in stream works .
- The operations are executed in sequential manner for each individual elements and not the entire stream .
- However , there are some operations which require complete stream to be present and cannot work with single elemengt . eg Sorted.

- SO in the boave scenario , 2 , 1 was rejected because it doesnt fulfill the first filter criteria(>=3).
- As sson as 4 came in , it passed the first operation , and now instead of executing same with 7 , 4 goes further to next operation map(* -1) and becomes( -4 ) .
- But it didnt move further inside sorted because sorted needs whole stream to creater before its execution .
- Hence , the actual output.

- Summary

- ![image](https://github.com/user-attachments/assets/16c2214f-c1e1-4992-b35b-2d84a6d4e70d)



 <h2>Different Terminal Operations</h2>


 ![image](https://github.com/user-attachments/assets/7a33531a-77a1-4025-b600-96648f3ff191)


![image](https://github.com/user-attachments/assets/83b1f33a-5109-4e11-8c32-e380daf591c8)


![image](https://github.com/user-attachments/assets/513fd642-d7dc-4f76-9c5d-cb475453c106)

Assosciate aggregator function means - 

![image](https://github.com/user-attachments/assets/d320808b-2fb4-4fa2-84ec-62bcc3d74e5e)


![image](https://github.com/user-attachments/assets/8b910878-c3a7-47a1-a6ab-72469d94e9ca)

- min(Comparator<T> comparator) and max(Comparator<T> comparator)

![image](https://github.com/user-attachments/assets/6362a235-a41d-4f7d-8923-5cd43c0a77a2)


![image](https://github.com/user-attachments/assets/1d6b585f-4419-412b-9cf0-606ef98f1951)


![image](https://github.com/user-attachments/assets/fb883a50-a12d-4429-b6f2-a9fae6f4c159)

- Similary ,we have allMatch and NoneMatch 

![image](https://github.com/user-attachments/assets/41aca9b8-03aa-46ca-ade4-7ce1704cf5b5)


![image](https://github.com/user-attachments/assets/daa66a3e-bb33-458a-bb4c-b12871a17589)


- Untill now we saw , different intermediate operations and their sequence and different terminal operations and its uses.


![image](https://github.com/user-attachments/assets/78e8653d-9bf6-4024-88ba-3ff4ce1ebcc2)


![image](https://github.com/user-attachments/assets/0a123a81-c6b0-42ab-82b0-dd89ee3a28d8)



<h2>Parallel Stream</h2>


![image](https://github.com/user-attachments/assets/a985598b-5517-4355-806f-7e574bccb9fb)

![image](https://github.com/user-attachments/assets/dbaf7d6c-e2f8-434a-81a1-2d863cfb80d8)


![image](https://github.com/user-attachments/assets/9de6c9f1-7a2b-4f9f-896a-d596a61c77c1)


![image](https://github.com/user-attachments/assets/cbdcf851-be49-4f1b-b196-c843f4a97978)



![image](https://github.com/user-attachments/assets/3b1b0d29-2563-4750-8484-5976b8ef404d)
- each subtask can be run concurrently

