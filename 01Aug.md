I learnt about Stack and Heap Memory and Garbage Collector in Java.
Credits - Concept and Coding (Shreyansh Jain)  - (https://www.youtube.com/playlist?list=PL6W8uoQQ2c63f469AyV78np0rbxRFppkx) 


 ![image](https://github.com/user-attachments/assets/f10ca3fe-fab2-48d5-84d3-acbeaf6bc3bf)
 ![image](https://github.com/user-attachments/assets/d13cf9d1-6727-42b1-91b5-48833fca5d33)
![image](https://github.com/user-attachments/assets/d7cd3d90-3e17-4933-8112-d40f4a4c4898)

![image](https://github.com/user-attachments/assets/3a72e2c6-cf17-4d1c-8d60-32994b913c1c)
![image](https://github.com/user-attachments/assets/bdc58bd7-746d-436c-8ef4-420e3af6096d)
![image](https://github.com/user-attachments/assets/fe60ee78-b647-4620-b4ba-64b633c3d880)

 
JVM controls when to run the garbage collector. 

AUtomatically garbage collector.

To run garbage collector from code we can do System.gc() in code to run garbage collector. There is no guarantee that jvm will run it that time , its just a request to JVM to run.

 

Strong reference - whenever gc runs it will check whether object is pointing by reference , it will not delete .
 
Weak reference - wheneebr gc runs it will delete the object even if it is getting pointed by a weak reference . It is only avaialble until gc is run again .
to create a weak reference there is inbuilt class WeakReference


WeakReference<Person> weakpobj = new WeakReference<Person>(new Person());

This will create a weak reference as stated on the image .

Once it is deleted by gc , the reference will store null.

 
 

Soft reference - type of weak reference it states gc that , gc can remove the object pointed by it only when it is very urgent.(no more space).

weak - as ssoon as gc runs , remove the object .
soft - only remove when its urgent.


Heap memory : 
divided into two parts youung generation and old generation.
one more part - not heap (called as metaspace)

 

 

s0 , s1 - survivor space

whenever a new object “obj1” is created it is saved on eden space as seen on image.

 

Lets create more objects obj2,3,4,5.
 
So periodically when gc runs it uses mark and sweep algorithm.
a first step it will mark objects which are not pointed by reference and allowed to be deleted.

lets say e.g obj2 and obj5 are those.
  
then comes sweep algorithm.
it will remove the marked object o2 and o5 and move(sweep) the other surviving objects 01 , o3 and o4 to one of the survivors memory.

 
and age is added , e.g age is 1. means age is increasing.

So this is minor gc , as it happens very periodically and very fast.

now second time , another objects created , obj6 and obj7 .
it will get created to eden space as new objects get stored in eden only.

now if gc runs .
 
if obj7 and obj4 is marked to be deleted.

Sweep ,. what will do is remove obj7 and obj4 form memory and sweep surviving objects in s1 , (it uses alternatively). and increases teh age .

 
 
now we can see , whenever gc runs we get eden and one of so and s1 freed , a;lternatively we put data in s1 and s0 and we thus we incerease the range.

if we set threshold to age=3 , it has to promote them.

create two new obj and again gc runs for the third time .

 

it marks o8 and o3 , then sweep ,
removes the o8 and o3 and moves other objects o9 o6 o1 to s0, with age 1 ,2, 3 respectively.

 

 
now o1 reached trheshold , e.g it survived for age 3 , it has to promote (or move the old generation).

 

 
in this space gc runs less perioadically than new generation space. and its major gc.
and objects in old generations they seem to be imp objects as it is used too frequently .

 
in old gen , gc is time taking as it is alive for many time , it could have many references.

Now metaspace ?
Its outside of heap , it will store class variables. (static variables).
It will store class metadata , stores class constant.
When JVM loades certain clas it will load it here .

old gen is also called tenure.
 
earlier before i guess java 7 , this metaspace was called as premjam , 
difference was , beffore java 7 premjam was part of heap and if premjam gets filled up it will get out of memory error.

Now its gets seperated and is expandable, kind of data stored is same.

Garbage collector :
1 -mark and sweep algo
2- mark and sweep with compaction memory 
 
if blue ones are marked and got deleted by sweep , there is space remaining in between the segments , so compaction does is , arrange the empty spaces and filled spaces in order ., so we have sequential space.
Free space can we used to put objects.

Different versions of gc(versions)

serial gc , 
Only 1 thread is used for gc . 
only 1 thread for minor gc and 1 thread for major gc 
disadvantage : 
--slow 
--gc is very very expensive , because whenever gc is stasrted all other app threads will get paused. 
if gc is slow , app will get slow. 
parallel gc(default in java8) , 
in this depending upon core , 2 core cpu , 4 core cpu , this gets alloted to gc and it works paralelly. 
Less pause time. still there is pause time.
concurrent mark and sweep , (java 17)
says that , while app threads are working , concurrently gc threads will also work. So app threads will not pause when gc threads are also working concurrently , but its not 100% guarantee.
JVM will try to do concurrent , but its not guaranteed , and no compaction happens .
g1 garbage collector.(better version)(latest java)
100% try to give app thread will not stop , and compaction happens
so thorughput increases and latency decreases.
 

