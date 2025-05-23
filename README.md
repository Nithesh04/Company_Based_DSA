# Company_Based_DSA
### Q1: You are working as a software developer for financial analytics firms. One of your models receives an array of daily stock performance indicators. Positive numbers indicate profitable days, negative numbers indicate lost days. To improve the model, one should alternate profit and loss days in the report. If there are more positive or more negative days, the excess should be placed at the end in the same order. write a c++ function to rearrange the given array so that positive and negative numbers appear alternatively. the relative order of elements should be preserved as much as possible. testcase1: 5,-1,3,2,-7,8,9   output: 5,-1,3,-7,2,8,9 test case 2: -3,-2,4,-1,-6,7  output: -3,4,-2,7,-1,-6

    void rearrangeAlternating(vector<int>& arr) {

        vector<int> positives, negatives;
        
        // Step 1: Separate positive and negative numbers
        for (int num : arr) {
            if (num >= 0)
                positives.push_back(num);
            else
                negatives.push_back(num);
        }
    
        // Step 2: Merge alternately
        int i = 0, p = 0, n = 0;
        vector<int> result;
        while (p < positives.size() && n < negatives.size()) {
            result.push_back(positives[p++]);
            result.push_back(negatives[n++]);
        }
    
        // Step 3: Add remaining positives
        while (p < positives.size())
            result.push_back(positives[p++]);
    
        // Step 4: Add remaining negatives
        while (n < negatives.size())
            result.push_back(negatives[n++]);
    
        // Step 5: Copy result back to original array
        arr = result;
}

### Q2: You are developing a flight optimization system for a major travel booking company. Given a list of available flights, your task is to help users find the longest sequence of connecting flights so they can take in a single trip. Each flight is represented as departure time, arrival time in 24-hour format. The user can take only the next flight if its departure time is strictly of the arrival time of the previous flight and there must be at least one hour of layover time between flights. For example, next departure is greater than previous arrival plus one. The flights are not to be shorter. You are to return the maximum number of flights the user can book under this book. An integer matrix int flight where flights of i is equal to departure arrival for each flight. Return an integer the maximum number of valid connecting flights the user can take in one journey. Test case 1. Flights is equal to [(2, 5), (3, 6), (7, 8), (8, 10), (12, 14)]. output: 3.  test case2: [(5, 7), (6, 9), (8, 10), (9, 11), (10, 13)]. output: 2.
    int maxConnectingFlights(vector<vector<int>>& flights) {
        int n = flights.size();
        if (n == 0) return 0;
    
        // Sort by departure time
        sort(flights.begin(), flights.end());
    
        vector<int> dp(n, 1);  // Each flight can be taken individually
    
        int maxFlights = 1;
    
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                // Valid connection: next dep > prev arr + 1
                if (flights[i][0] > flights[j][1] + 1) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            maxFlights = max(maxFlights, dp[i]);
        }
    
        return maxFlights;
    }

### Q3: Users can add money to their wallet and choose from their list of available item prices. The application should use us to find all unique combinations of item prices that exactly boost up their budget. However, users can buy the same item multiple times, e.g. buying two packets of rice, price Rs. 5 is allowed. Given an array of positive integer prices representing item prices and an integer budget, return all unique combinations when the chosen item price sums up to the budget. You can use each item unlimited times. Test Case 1 Prices :[2,5,3] Budget 8 output: [[2,2,2,2],[3,3,2],[5,3]] Test Case 2 Prices :[8,9,1,0] Budget 7 output:[] Test Case 3 :Prices :[1] Budget 2 Answer [[1,1]]
//backtracking

    void backtrack(vector<int>& prices, int budget, vector<int>& current, vector<vector<int>>& result, int start) {
        if (budget == 0) {
            result.push_back(current);
            return;
        }
        for (int i = start; i < prices.size(); ++i) {
            if (prices[i] > budget) continue;
            current.push_back(prices[i]);
            backtrack(prices, budget - prices[i], current, result, i);  // reuse same item
            current.pop_back();  // backtrack
        }
    }
    vector<vector<int>> findCombinations(vector<int>& prices, int budget) {
        vector<vector<int>> result;
        vector<int> current;
        sort(prices.begin(), prices.end());  // optional but helps avoid unnecessary work
        backtrack(prices, budget, current, result, 0);
        return result;
    }
//DP

    void buildCombinations(int target, vector<vector<vector<int>>>& dp, vector<vector<int>>& result) {
    result = dp[target];
    }

    vector<vector<int>> combinationSumDP(vector<int>& prices, int budget) {
        sort(prices.begin(), prices.end());
        prices.erase(remove_if(prices.begin(), prices.end(), [](int x){ return x <= 0; }), prices.end());
    
        // dp[i] stores all combinations that sum to i
        vector<vector<vector<int>>> dp(budget + 1);
    
        // base case: one way to make 0 sum → empty list
        dp[0] = {{}};
    
        for (int price : prices) {
            for (int sum = price; sum <= budget; ++sum) {
                for (const auto& combo : dp[sum - price]) {
                    vector<int> newCombo = combo;
                    newCombo.push_back(price);
                    dp[sum].push_back(newCombo);
                }
            }
        }
    
        vector<vector<int>> result;
        buildCombinations(budget, dp, result);
        return result;
    }

### Q4: Climbing Stairs
### You are climbing a staircase. It takes n steps to reach the top.
### Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Example 1:
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
   
Example 2:
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

#### tabulation
        int climbStairs(int n) {
            if (n == 1) return 1;
        
            int dp[n + 1];  // create DP array
            dp[1] = 1;
            dp[2] = 2;
        
            for (int i = 3; i <= n; ++i) {
                dp[i] = dp[i - 1] + dp[i - 2];
            }
        
            return dp[n];
        }

#### Space optimization
        int climbStairs(int n) {
            if (n == 1) return 1;
            int first = 1, second = 2;
            for (int i = 3; i <= n; ++i) {
                int third = first + second;
                first = second;
                second = third;
            }
            return second;
        }

### Q5: A frog wants to cross a river by jumping across n stones arranged in a line. the frog starts at stone 0 and wants to reach stone n. the flag can jump either 1,2 or 3 stones forward at a time. our task is to find how many different ways to frog can reach the nth stone 

#### tabulation
        int countWays(int n) {
            vector<int> dp(n + 1, 0); // Create a dp array of size (n+1), initialized to 0
            dp[0] = 1; // There's 1 way to stand at the starting stone (doing nothing)
        
            for (int i = 1; i <= n; i++) {
                if (i >= 1) dp[i] += dp[i - 1];
                if (i >= 2) dp[i] += dp[i - 2];
                if (i >= 3) dp[i] += dp[i - 3];
            }
        
            return dp[n]; // Return the total number of ways to reach the nth stone
        }
#### Space optimization
    int countWays(int n) {
        if (n == 0) return 1;
        if (n == 1) return 1;
        if (n == 2) return 2;
    
        int a = 1; // dp[0]
        int b = 1; // dp[1]
        int c = 2; // dp[2]
        int current;
    
        for (int i = 3; i <= n; i++) {
            current = a + b + c; // dp[i] = dp[i-1] + dp[i-2] + dp[i-3]
            a = b;
            b = c;
            c = current;
        }
    
        return c;
    }

### Q6 : Coin Change
### You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.
### Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
### You may assume that you have an infinite number of each kind of coin.
Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0
#### tabulation
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1);  // Initialize dp array with a large number (amount+1)
        dp[0] = 0;  // Base case: zero coins needed to make amount 0
    
        for (int coin : coins) {
            for (int x = coin; x <= amount; ++x) {
                dp[x] = min(dp[x], dp[x - coin] + 1);
            }
        }
    
        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }

