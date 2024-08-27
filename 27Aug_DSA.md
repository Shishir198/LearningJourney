<h1>Majority Sum - Leetcode EASY</h1>

Today I solved this question  - https://leetcode.com/problems/majority-element

Althoguh the question is easy , and different approach of solving the problem using hashmaps etc.

Twist happens where we need to the same with O(n) time complexity with O(1) space complexity.

SO , in order to solve this in constraint I learn about <h2>Moore Voting Algorithm</h2> . 

Intuition : 

Just think if we have array [1,1,1,2,2] , 1 is majority element but we can see power of 1 is more as observe in subarray from index 1 to last : [1,1,2,2]
We can say , all can get cancelled with each other 1 cancels 2 , another 1 cancels another 2 . And therefore only 1 remains because it is highest .

Similary , in another examples we can observe numbers can fight between themselves i.e cancel their enemy(different number) and the last man standing will be our majority element.

[1,1,1,2,3]  ---> observe [1,1,2,3] 1 can cancel 2 , another 1 will cancel 3 as its different element . so remains - [1] .

<h2>Implementation</h2> :
Implementation is moore voting algorithm.
We first assume that ifrst element in majority elem and count its frequency as 1 .
If it encounters different elements , we can decrease its power (frquency).

If freq , reaches 0 that means some other number overpowered it , so the new number will be assumed to be next majoruty elem and same thing is repeated.

```
    public int majorityElement(int[] nums) {
            int number = nums[0];
            int frequency = 1;
            for(int i=1;i<nums.length;i++){
            if(frequency == 0){
                number = nums[i];
                frequency++;
                continue;
            }

            if(nums[i] == number){
                frequency+=1;
            }
            else{
                frequency-=1;
            }
        }

        return number;
        
    }
    ```

