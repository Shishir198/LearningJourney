<h1>Best Time to Buy and Sell Stock II</h1>

Problem link - https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/

Intuition - As we can but and sell any no of time , we can take a greedy approach here .
Whenever , price gets increasing - wait  , As soon as price gets decreased than the last day , BUY and then repeat again.

```
public int maxProfit(int[] prices) {
        
        int profit = 0;
        int i = 0;

        for(int j=0;j<prices.length;j++){
            if(prices[i] < prices[j]){
                profit+= prices[j] - prices[i];
            }
            i=j;
        }

        return profit;

    }
```


<h1>Jump Game</h1>


Problem Link - https://leetcode.com/problems/jump-game/


Intuition - Simply did brute force using recursion and then saved the intermeditate result using dp array (Dynamic Programming concept).

```
public int canJumpHelper(int pos , int[] nums , int[] dp){
        if(dp[pos]!=-1){
            return dp[pos];
        }
        if(pos == nums.length-1){
            return 1;
        }

        int jumpAvailable = nums[pos];
        int res = 0;
        for(int i=1;i<=jumpAvailable;i++){
            if(pos+i <= nums.length-1){
                int i_res = canJumpHelper(pos+i , nums , dp);
                dp[pos+i] = i_res;
                if(i_res == 1){
                    res = 1;
                    break;
                }
            }
        }
        dp[pos] = res;
        return res;
    }

    public boolean canJump(int[] nums) {

        int[] dp = new int[nums.length];
        for(int i=0;i<nums.length;i++){
            dp[i] = -1;
        }

        return canJumpHelper(0 , nums , dp)==1 ? true : false;
        
    }
```
