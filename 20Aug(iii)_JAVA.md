Concurrency can be achieved using both ways

![image](https://github.com/user-attachments/assets/50343a2a-4bb4-42b9-8b93-712070c36990)

- Lock free will be faster , but its not a alternative.
- It only used in specific use case areas
- Cas Operation (compare and Swap) and Java provide these four classes which uses this technique
- It's like optimistic concurrency control , optimistic locking
- Like , when T1 reads the data and after read check whether its been updated or not , if updated it again reads and then try to re-update.

  ![image](https://github.com/user-attachments/assets/244434e1-778a-4cbd-95c0-c3cb99dffcf5)

  how it wroks : 
It takes 3 parameters

CAS(memory , expected , new ){
1 - it will check on the memory location .
2 - it will compare whether the data on memory is equal to expected value.
    2.1 - IF not equal then something is changed 
3 - If equal it will update it with new value.
}


ABA problem : 

![image](https://github.com/user-attachments/assets/16acee79-75c3-4a0a-8a35-62277de492e7)

Lets say , meemory x = 10 we want to update what cas will do 
CAS will read m1 , compare with expected value=10, if equals it will update with 13 only when its not changed in meanwhile.

But , what if thread2 might have changed the value from 10 to 12 and then again to 10 . T1 will check and confirm okay this is 10 and so not yet changed , but is it the case ?? no 
This is ABA problem

Resolved using version with memory or timestamp  ,

x= 10 , v=1 
x = 12 , v=2 
x = 10 , v=3 .

chcek CAS with (memory , expected [10 with version1], new)


<h2>Atomic Variables</h2>

- means not breakable further
- single , all or nothing

- if count = 10  , not matter how many threads will excute this it will be executed in single operation

- but counter ++ is not atomic because it goes through several operations to achieve the objective to add 1.
- 1)Loads the counter value
- 2)Increment by 1
- 3)Set the value

- Not a single operation , thread can be preempted in between, which can cause inconsistency (not thread safe)

- To make it thread safe we make it synchonised

- ANother solution atomic variables (lock free solution)

- Atomic variables internslly uses CAS.
  
![image](https://github.com/user-attachments/assets/65900498-159b-4eda-b917-6a00c5767d85)

- NOtice not dping counter++ , but incrementAndGet()
- It internally uses
- ![image](https://github.com/user-attachments/assets/5eea5c38-1d67-42dd-877c-035704e8d7ba)

- Atomic can be used in
- read , modify and update use cases

- ![image](https://github.com/user-attachments/assets/d7631c9b-076c-4033-8164-f51738b28455)


![image](https://github.com/user-attachments/assets/f90530a2-336b-4a33-aa33-97995850cac9)

- It will only update the value when CAS operation is true i.e , expected is found
- else , it will again continue in loop and then check again with the updated value


Atomic vs Volatile 

- Atomic
- provides thread safety
- 

- Volatile
- No relation with threas safety
- it sjust states that , dont read/write from l1 cache of threads present on cpu core , do operations directly on memory

  <h2>COllections and there thread safe alternatives</h2>

  ![image](https://github.com/user-attachments/assets/dd2e5de7-3975-4197-8823-ed99237accc3)




