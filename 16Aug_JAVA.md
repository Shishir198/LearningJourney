Singleton Classes and Immutable classes


Credits - : https://www.youtube.com/watch?v=SqDbZOjW1uM | Shreyansh Jain  Concept and Coding.

![image](https://github.com/user-attachments/assets/fd88565c-3fcf-4dbb-8c82-c78278553057)

When we want to create only one object of class throughtout the project.

E.g Database connections
- we want to establish connection with db only first time , then reuse the connection for different requests (select , update , delete etc);

![image](https://github.com/user-attachments/assets/9489a899-2888-4551-bafb-de419a8c9b83)

I created an object of this eagerly in advance itself so I have make it private static so what why I
made it private this object because I don't want anybody to access it apart from this class right I don't want anybody 
to access it and change it also right so private is uh that one that it should it can be accessed only within this class and 
static means static means only so this is not related to object now this is related to class right so this object con object 
is now belongs to a class not belongs to an object.

I made the Constructor private so that nobody is allowed to create a new object through new keyword outside of this class

This is eager init , because as soon as application is start static variables are preloaded into the memory
i,e , eventhough somebody doesnt need this , didnt called getInstance, still it will be created into the memory .

I created a public method now so that others if they wanted an object of this class they can call this method and I will just return this object 
which I have created and I have created a static so that they can call with the class name itself so this method belongs to a class also so
call this method like this

![image](https://github.com/user-attachments/assets/af7a3cb0-da2f-4b2a-88f7-d057c6690b28)



Lazt init : 
![image](https://github.com/user-attachments/assets/ee0dc210-2a6a-47c8-88ab-0ece5757879e)

Object is being created whenever its needed , whenever someone calls the function getInstance();


But its not thread proof.

If at same time , two threads reached and read conObjet == null ? - true ,. both threads will create a new object.


Solution - Synchronized method

![image](https://github.com/user-attachments/assets/ddebc368-f9e4-433b-b6e2-594e732f1df8)


I have put out synchronize now let's say two thread are calling this get instance 
and this is also calling get instance now only one thread will be allowed to go inside this block


only disadvantage of this is that this we are putting a synchronize at a method level so this is becoming 
a very slow why let's say there are so many places we are calling get instance we are we are calling get instance 
at so many places.

Even if onject is already created still , synchronised block will manage assigning lock , removing lock from threads whoever 
wants to just access the method.

We should only create such scenario , only when we are going to create a new object.
Otherwise , uncessarily only for checking the object is created , we have to manage locks.

Solution  : 
double locking

![image](https://github.com/user-attachments/assets/65598076-c871-4801-93b7-8a329d195d88)


Two threads comes at the same time T1 and T2 so both will find it is null yes they will come inside it now 
because of the synchronized only one of them will go and one of them will Acquire The Log and go ahead so 
let's say T1 goes ahead now T1 is checking again hey is it still a null only right yes it is still a null and 
it will go and create an object .

Now after it release the lock now thread 2 will come here and it will now again a double check hey it is still 
a null right , no it is not null now object has been created so it will not create new object ,it will simply return.

Volatile - 
volatile purpose is that if you have created any object volatile any read and write happening to 
this is always happen from memory not from the cache . 
In order to reflect change of object creation globally to all threads , we use volatile.


This double check locking is slow because we are using volatile which means it is not using cache so definitely it's 
little bit slow and there's also a synchronized keyword we are using so this is also putting lock and lock so this is 
little bit slow to overcome this there is one more solution called Bill Puck solution.

Bill Puck Solution  :

![image](https://github.com/user-attachments/assets/4a41ccc2-dd0c-4b1e-a479-024762426d0e)


it is making use of this eager initialization right what does the disadvantage of eager initialization is hey even this 
class is not even used object is created right equally we have initialized number so what does Bill Puck has done is resolve 
that only it the initialization part private static one right it just put it inside nested class a static nested class 
private static nested class why because this nested class do not get loaded at the time of application when it you start the application all the
nested class do not get loaded into the memory only when they are referred when they are used then only it will load.

it is very fast and this is I think you know you can tell this is also a popular 


ENUM : 

![image](https://github.com/user-attachments/assets/e5f7155a-d368-4641-9b7d-15e47cccd0f3)


in enum everything is happen automatically so enum Constructors are always by default private.
the object of INSTANCE , its enum take care that only one instance of exist per jvm one instance only exists per jvm right so that's what 
Singleton is right one object per jvm okay so if you see that enum solve it within two lines itself where we are writing so many method.



IMMUTABLE  CLASS:

![image](https://github.com/user-attachments/assets/8a2b3e14-c338-41f4-aafa-f042a06391bc)

![image](https://github.com/user-attachments/assets/fb715cd3-0d20-4e50-a369-386ba42ad3a5)

I've created a class as a final because I don't want any subclass to be created and change the value .

I have created a private and final means nobody can access this variables outside of this class right 
and once the value has been assigned to it they can't be changed that's what finals say right.

class members are initialized only once using Constructor so whenever you create an object of it that time 
you pass what value you wanted to assign to the variables that time you can allow after that the value 
should not be changed.

No setter method alloweed.

return the copy of the member variable , because the original variables should not be changed.

Inside the list , it is allowed to change , but the reference should be constant , reference should not be changed to different list












