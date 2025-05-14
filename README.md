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
