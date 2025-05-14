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
