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
### Day 01 - Corporate - Technical Questions 
### There is a JAR full of candies for sale at a mall counter. JAR has the capacity N, that is JAR can contain maximum N candies when JAR is full. At any point of time. JAR can have M number of Candies where M<=N. Candies are served to the customers. JAR is never remain empty as when last k candies are left. JAR if refilled with new candies in such a way that JAR get full.

Write a code to implement above scenario. Display JAR at counter with available number of candies. Input should be the number of candies one customer can order at point of time. Update the JAR after each purchase and display JAR at Counter.

Output should give number of Candies sold and updated number of Candies in JAR.

If Input is more than candies in JAR, return: “INVALID INPUT”

Given,

N=10, where N is NUMBER OF CANDIES AVAILABLE
K =< 5, where k is number of minimum candies that must be inside JAR ever.

Example 1:(N = 10, k =< 5)

Input Value
3
Output Value
NUMBER OF CANDIES SOLD : 3
NUMBER OF CANDIES LEFT : 7

Example 2 : (N=10, k<=5)
Input Value
0
Output Value
INVALID INPUT
NUMBER OF CANDIES LEFT : 10

    #include <iostream>
    using namespace std;

    int main() {
        const int N = 10; // Maximum capacity of the jar
        const int K = 5;  // Minimum threshold before refill
        int jar = N;      // Initial candy count
    
        int input;        // Candies requested by the customer
    
        // Read input from user
        cout << "Enter the number of candies to buy: ";
        cin >> input;
    
        // Check for invalid input
        if (input <= 0 || input > jar) {
            cout << "INVALID INPUT" << endl;
            cout << "NUMBER OF CANDIES LEFT : " << jar << endl;
        } else {
            // Sell candies
            jar -= input;
            cout << "NUMBER OF CANDIES SOLD : " << input << endl;
    
            // Refill jar if candies fall below or equal to K
            if (jar <= K) {
                jar = N;
            }
    
            cout << "NUMBER OF CANDIES LEFT : " << jar << endl;
        }
    
        return 0;
    }
### Day 02 - Corporate - Technical Questions 

There are total n number of Monkeys sitting on the branches of a huge Tree. As travelers offer Bananas and Peanuts, the Monkeys jump down the Tree. If every Monkey can eat k Bananas and j Peanuts. If total m number of Bananas and p number of Peanuts are offered by travelers, calculate how many Monkeys remain on the Tree after some of them jumped down to eat.

At a time one Monkeys gets down and finishes eating and go to the other side of the road. The Monkey who climbed down does not climb up again after eating until the other Monkeys finish eating.
Monkey can either eat k Bananas or j Peanuts. If for last Monkey there are less than k Bananas left on the ground or less than j Peanuts left on the ground, only that Monkey can eat Bananas(<k) along with the Peanuts(<j).

Write code to take inputs as n, m, p, k, j and return  the number of Monkeys left on the Tree.
Where, n= Total no of Monkeys
k= Number of eatable Bananas by Single Monkey (Monkey that jumped down last may get less than k Bananas)
j = Number of eatable Peanuts by single Monkey(Monkey that jumped down last may get less than j Peanuts)
m = Total number of Bananas
p  = Total number of Peanuts

Remember that the Monkeys always eat Bananas and Peanuts, so there is no possibility of k and j having a value zero

Example 1:

Input Values    
20
2
3
12
12


Output Values

Number of  Monkeys left on the tree:10

Note: Kindly follow  the order of inputs as n,k,j,m,p as given in the above example. And output must include  the same format  as in above example(Number of Monkeys left on the Tree:)

For any wrong input display INVALID INPUT

    #include <iostream>
    using namespace std;
    
    int main() {
        int n, k, j, m, p;
    
        // Reading inputs
        cin >> n >> k >> j >> m >> p;
    
        // Check for invalid input
        if (n <= 0 || k <= 0 || j <= 0 || m < 0 || p < 0) {
            cout << "INVALID INPUT" << endl;
            return 0;
        }
    
        int monkeysLeft = n;
        
        for (int i = 0; i < n; ++i) {
            // Check if enough for a full banana meal
            if (m >= k) {
                m -= k;
                monkeysLeft--;
            }
            // Else, check if enough for a full peanut meal
            else if (p >= j) {
                p -= j;
                monkeysLeft--;
            }
            // Last monkey can eat remaining smaller quantity of both
            else if (m > 0 || p > 0) {
                m = 0;
                p = 0;
                monkeysLeft--;
                break; // no more food left after this
            }
            // No food left
            else {
                break;
            }
        }
    
        cout << "Number of Monkeys left on the tree:" << monkeysLeft << endl;
        return 0;
    }
### Day 03 - Corporate - Technical Questions  

A carry is a digit that is transferred to left if sum of digits exceeds 9 while adding two numbers from right-to-left one digit at a time

You are required to implement the following function.

Int NumberOfCarries(int num1 , int num2);

The functions accepts two numbers ‘num1’ and ‘num2’ as its arguments. You are required to calculate and return  the total number of carries generated while adding digits of two numbers ‘num1’ and ‘ num2’.

Assumption: num1, num2>=0

Example:

Input

Num 1: 451
Num 2: 349

Output
2

    int NumberOfCarries(int num1, int num2) {
        int carry = 0;
        int count = 0;
    
        while (num1 > 0 || num2 > 0) {
            int digit1 = num1 % 10;
            int digit2 = num2 % 10;
    
            int sum = digit1 + digit2 + carry;
    
            if (sum >= 10) {
                carry = 1;
                count++;
            } else {
                carry = 0;
            }
    
            num1 /= 10;
            num2 /= 10;
        }
    
        return count;
    }

### Day 03 - Corporate - Technical Questions  
Particulate matters are the biggest contributors to Delhi pollution. The main reason behind the increase in the concentration of PMs include vehicle emission by applying Odd Even concept for all types of vehicles. The vehicles with the odd last digit in the registration number will be allowed on roads on odd dates and those with even last digit will on even dates.

Given an integer array a[], contains the last digit of the registration number of N vehicles traveling on date D(a positive integer). The task is to calculate the total fine collected by the traffic police department from the vehicles violating the rules.

Note : For violating the rule, vehicles would be fined as X Rs.

Example 1:

Input :

4 -> Value of N

{5,2,3,7} -> a[], Elements a[0] to a[N-1], during input each element is separated by a new line

12 -> Value of D, i.e. date 

200 -> Value of x i.e. fine

Output :

600  -> total fine collected

    int calculateFine(int a[], int n, int d, int x) {
        int totalFine = 0;
        bool isDateEven = (d % 2 == 0);
    
        for (int i = 0; i < n; i++) {
            bool isVehicleEven = (a[i] % 2 == 0);
    
            // Fine if vehicle parity doesn't match date parity
            if (isVehicleEven != isDateEven) {
                totalFine += x;
            }
        }
    
        return totalFine;
    }

### Day 05 - Corporate - Technical Questions  

You are given a function,

Int MaxExponents (int a , int b);

You have to find and return the number between ‘a’ and ‘b’ ( range inclusive on both ends) which has the maximum exponent of 2.

The algorithm to find the number with maximum exponent of 2 between the given range is

Loop between ‘a’ and ‘b’. Let the looping variable be ‘i’.
Find the exponent (power) of 2 for each ‘i’ and store the number with maximum exponent of 2 so faqrd in a variable , let say ‘max’. Set ‘max’ to ‘i’ only if ‘i’ has more exponent of 2 than ‘max’.
Return ‘max’.
Assumption: a <b

Note: If two or more numbers in the range have the same exponents of  2 , return the small number.

Example

Input:

7
12

Output:
8

    // Helper function to count how many times 2 divides a number
    int countExponentOf2(int n) {
        int count = 0;
        while (n % 2 == 0) {
            n /= 2;
            count++;
        }
        return count;
    }
    
    // Main function to find the number with the maximum exponent of 2
    int MaxExponents(int a, int b) {
        int maxExponent = -1;
        int result = a;
    
        for (int i = a; i <= b; i++) {
            int exp = countExponentOf2(i);
            if (exp > maxExponent) {
                maxExponent = exp;
                result = i;
            } else if (exp == maxExponent && i < result) {
                result = i;  // pick the smaller number in case of tie
            }
        }
    
        return result;
    }

### Day 06 - Corporate - Technical Questions
You are given a function,

Void *ReplaceCharacter(Char str[], int n, char ch1, char ch2);

The function accepts a string  ‘ str’ of length n and two characters ‘ch1’ and ‘ch2’ as its arguments . Implement the function to modify and return the string ‘ str’ in such a way that all occurrences of ‘ch1’ in original string are replaced by ‘ch2’ and all occurrences of ‘ch2’  in original string are replaced by ‘ch1’.

Assumption: String Contains only lower-case alphabetical letters.

Note:

Return null if string is null.

If both characters are not present in string or both of them are same , then return the string unchanged.

Example:

Input:

Str: apples
ch1:a
ch2:p

Output:
paales

#### Purpose: Modifies the string in-place, and also returns a pointer to the modified string.
Usage: Lets the caller receive the modified string explicitly, even if they use the return value.

    void* ReplaceCharacter(char str[], int n, char ch1, char ch2) {
        // Null check
        if (str == nullptr) return nullptr;
    
        // If both characters are the same, return as-is
        if (ch1 == ch2) return str;
    
        // Flags to check if ch1 or ch2 are present in the original string
        bool foundCh1 = false, foundCh2 = false;
    
        // First pass to detect if either ch1 or ch2 exists
        for (int i = 0; i < n; i++) {
            if (str[i] == ch1) foundCh1 = true;
            else if (str[i] == ch2) foundCh2 = true;
        }
    
        // If neither character is present, return unchanged string
        if (!foundCh1 && !foundCh2) return str;
    
        // Second pass to swap characters
        for (int i = 0; i < n; i++) {
            if (str[i] == ch1)
                str[i] = '#';  // Temporary placeholder to avoid conflict
        }
        for (int i = 0; i < n; i++) {
            if (str[i] == ch2)
                str[i] = ch1;
        }
        for (int i = 0; i < n; i++) {
            if (str[i] == '#')
                str[i] = ch2;
        }
    
        return str;
    }
#### Purpose: Modifies the string in-place and does not return anything.
Usage: You just call the function and it changes the original string directly.

    void ReplaceCharacter(char str[], int n, char ch1, char ch2) {
        if (str == nullptr || ch1 == ch2) return;
    
        bool foundCh1 = false, foundCh2 = false;
    
        for (int i = 0; i < n; i++) {
            if (str[i] == ch1) foundCh1 = true;
            else if (str[i] == ch2) foundCh2 = true;
        }
    
        if (!foundCh1 && !foundCh2) return;
    
        // Replace ch1 temporarily with #
        for (int i = 0; i < n; i++) {
            if (str[i] == ch1)
                str[i] = '#';
        }
    
        // Replace ch2 with ch1
        for (int i = 0; i < n; i++) {
            if (str[i] == ch2)
                str[i] = ch1;
        }
    
        // Replace # with ch2
        for (int i = 0; i < n; i++) {
            if (str[i] == '#')
                str[i] = ch2;
        }
    }

#### Day 08 - Corporate - Technical Questions 

Lowest Common Ancestor in BST

Given a Binary Search Tree and two node values, return their Lowest Common Ancestor (LCA).

BST:       6  
         /   \  
        2     8  


Input: p = 2, q = 8  

Output: 6

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root==NULL) return NULL;
        if(root->val<p->val && root->val <q->val){
            return lowestCommonAncestor(root->right,p,q);
        }
        if(root->val>p->val && root->val>q->val){
            return lowestCommonAncestor(root->left,p,q);
        }
        return root;
    }

#### The system needs to compute the final grade for each student after processing their midterm and final exam scores. The final grade is determined as follows:

The midterm score contributes 40% to the final grade.

The final exam score contributes 60% to the final grade.

You are given two lists:

One list contains midterm scores of students.

Another list contains final exam scores of students.

Your task is to write a function calculate_final_grades(midterm_scores, final_scores) that computes the final grade for each student and returns a list of their final grades. The grades should be rounded to two decimal places.

Test Case 01  :

Input:  [50, 70, 90], [60, 80, 100]
Output: [55.0, 75.0, 95.0]

Test Case 02  :

Input:  [95, 88], [70, 77]
Output: [80.0, 80.4]

    vector<double> calculate_final_grades(const vector<int>& midterm_scores, const vector<int>& final_scores) {
        vector<double> final_grades;
        int n = midterm_scores.size();
    
        for (int i = 0; i < n; ++i) {
            double final_grade = (midterm_scores[i] * 0.4) + (final_scores[i] * 0.6);
            final_grade = round(final_grade * 100) / 100.0; // rounding to 2 decimal places
            final_grades.push_back(final_grade);
        }
    
        return final_grades;
    }

#### Day 10 - Corporate - Technical Questions - 02.06.2025

A warehouse has multiple shelves represented by an array of weights.You must determine if you can split the array into two parts with equal sum.

You're given an array of integers, where each element represents the weight on a shelf.
You need to determine if it's possible to split the array into two parts such that the sum of the weights in both parts is equal.

Sample Input 01 :

int[] shelves = {1, 2, 3, 4, 6}

Sample Input 02 :

int[] shelves = {1, 2, 3}

    bool canSplitArray(const vector<int>& shelves) {
        int totalSum = 0;
        for (int weight : shelves) {
            totalSum += weight;
        }
    
        // If total sum is odd, we cannot split into two equal parts
        if (totalSum % 2 != 0) {
            return false;
        }
    
        int target = totalSum / 2;
        int prefixSum = 0;
    
        // Check if there exists a prefix with sum equal to target
        for (int i = 0; i < shelves.size() - 1; ++i) {
            prefixSum += shelves[i];
            if (prefixSum == target) {
                return true;
            }
        }
    }

#### Day 10 - Problem Solving for the day 

Sliding Window Maximum

Given an array nums[] and an integer k, return an array of the maximum value in each sliding window of size k.

nums = [1,3,-1,-3,5,3,6,7], k = 3

Output = [3,3,5,5,6,7]

    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            deque<int> dq;
            vector<int> ans;
            int n=nums.size();
            for(int i=0;i<=n-1;i++){
                if(!dq.empty() && dq.front()<=i-k){
                    dq.pop_front();
                }
                while(!dq.empty() && nums[dq.back()]<nums[i]){
                    dq.pop_back();
                }
                dq.push_back(i);
                if(i>=k-1) ans.push_back(nums[dq.front()]);
            }
            return ans;
        }
#### Day 11 - Problem Solving for the day 

Find All Duplicate Numbers
Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice. Return all duplicates without using extra space and in O(n) time.
nums = [4,3,2,7,8,2,3,1]
Output = [2, 3]

    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for (int i = 0; i < nums.size(); ++i) {
            int index = abs(nums[i]) - 1;
            if (nums[index] < 0) {
                res.push_back(abs(nums[i]));
            } else {
                nums[index] = -nums[index];
            }
        }
        return res;
    }

#### Day 12 - Problem Solving for the day - 05.06.2025 

Subarray Sum Equals K

Given nums[] and an integer k, return the total number of continuous subarrays whose sum equals to k.

nums = [1,1,1], k = 2

Output = 2

    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> prefixSumCount;
        int currentSum = 0;
        int count = 0;
    
        prefixSumCount[0] = 1; // base case: prefix sum 0 occurs once
    
        for (int num : nums) {
            currentSum += num;
    
            if (prefixSumCount.find(currentSum - k) != prefixSumCount.end()) {
                count += prefixSumCount[currentSum - k];
            }
    
            prefixSumCount[currentSum]++;
        }
    
        return count;
    }

#### Day 13 - Problem Solving for the day 

Minimum Swaps to Sort

Given an array of distinct elements, find the minimum number of swaps required to sort the array.

Input:

arr = [4, 3, 2, 1]

Output: 2

    int minSwaps(vector<int>& arr) {
        int n = arr.size();
        vector<pair<int, int>> vec(n);
        
        // Store value and original index
        for (int i = 0; i < n; ++i)
            vec[i] = {arr[i], i};
        
        // Sort based on value
        sort(vec.begin(), vec.end());
        
        vector<bool> visited(n, false);
        int swaps = 0;
        
        for (int i = 0; i < n; ++i) {
            // If already visited or already in correct position
            if (visited[i] || vec[i].second == i)
                continue;
    
            int cycle_size = 0;
            int j = i;
            
            // Visit all nodes in this cycle
            while (!visited[j]) {
                visited[j] = true;
                j = vec[j].second;
                ++cycle_size;
            }
    
            // If cycle size is k, it takes (k-1) swaps
            if (cycle_size > 0)
                swaps += (cycle_size - 1);
        }
    
        return swaps;
    }

Day 14 - Problem Solving for the day 

Count Inversions in Array

An inversion is when arr[i] > arr[j] and i < j.Find the total number of inversions in an array.

Input: [2, 4, 1, 3, 5]

Output: 3

    int merge(vector<int>& arr, vector<int>& temp, int left, int mid, int right) {
        int inv_count = 0;
        int i = left;     // Starting index for left subarray
        int j = mid + 1;  // Starting index for right subarray
        int k = left;     // Starting index to be sorted
    
        while ((i <= mid) && (j <= right)) {
            if (arr[i] <= arr[j]) {
                temp[k++] = arr[i++];
            } else {
                temp[k++] = arr[j++];
                inv_count += (mid - i + 1); // Count inversions
            }
        }
    
        while (i <= mid) temp[k++] = arr[i++];
        while (j <= right) temp[k++] = arr[j++];
        for (int i = left; i <= right; i++) arr[i] = temp[i];
    
        return inv_count;
    }
    
    int mergeSort(vector<int>& arr, vector<int>& temp, int left, int right) {
        int mid, inv_count = 0;
        if (right > left) {
            mid = (left + right) / 2;
    
            inv_count += mergeSort(arr, temp, left, mid);
            inv_count += mergeSort(arr, temp, mid + 1, right);
            inv_count += merge(arr, temp, left, mid, right);
        }
        return inv_count;
    }
    
    int countInversions(vector<int>& arr) {
        vector<int> temp(arr.size());
        return mergeSort(arr, temp, 0, arr.size() - 1);
    }

#### Day 15 - Problem Solving for the day - 08.06.2025 

Maximum Length of Subarray With Equal 0s and 1s

Given a binary array, find the maximum length of a contiguous subarray with equal number of 0s and 1s.

Input: [0, 1, 0, 1, 1, 0]

Output: 6

    int findMaxLength(vector<int>& nums) {
        unordered_map<int, int> prefixMap; // sum -> first index
        int maxLength = 0;
        int sum = 0;
    
        // Initialize prefix sum 0 at index -1
        prefixMap[0] = -1;
    
        for (int i = 0; i < nums.size(); ++i) {
            // Convert 0 to -1
            sum += (nums[i] == 0) ? -1 : 1;
    
            if (prefixMap.find(sum) != prefixMap.end()) {
                maxLength = max(maxLength, i - prefixMap[sum]);
            } else {
                prefixMap[sum] = i;
            }
        }
    
        return maxLength;
    }

#### Day 16 - Problem Solving for the day
The Person using supermarket app needs to calculate the total cost of the items in a user's shopping cart after applying some discount rules.

The shopping cart is represented as an array of item prices.

Some items are eligible for a special discount. If the price of an item is greater than 50, a discount of 10% is applied to that item.

If the total cost of the cart exceeds 200, an additional 5% discount is applied to the entire cart.

Your task is to write a function calculate_cart_total(cart) that calculates the final total after applying the discounts.

Test Case 01  :

Input:  [10, 20, 30, 50]
Output: 110

    double calculate_cart_total(vector<double>& cart) {
        double total = 0.0;
    
        // Apply item-level discount
        for (double price : cart) {
            if (price > 50) {
                price *= 0.9; // 10% discount
            }
            total += price;
        }
    
        // Apply cart-level discount if total > 200
        if (total > 200) {
            total *= 0.95; // 5% discount
        }
    
        return total;
    }
#### Day 17 - Problem Solving for the day
Airline's Maintenance System.

After every flight, seats with even-numbered seat IDs need to be flagged for special cleaning due to proximity to air vents. The system receives a list of seat numbers that were occupied during the flight.

To help the cleaning crew, you must replace every even seat number in the list with 0 to indicate that those seats need to be cleaned.

Test case 01 :

Input:  [2, 4, 6, 8]
Output: [0, 0, 0, 0]

Test case 02 :

Input:  [12, 13, 14]
Output: [0, 13, 0]

    vector<int> flag_even_seats(vector<int>& seats) {
        for (int& seat : seats) {
            if (seat % 2 == 0) {
                seat = 0; // Even → mark for cleaning
            }
        }
        return seats;
    }

#### Day 18 - Problem Solving for the day - 11.06.2025

A flight booking system tracks the number of passengers who boarded at each stopover during a long journey with multiple cities. The data is stored in an integer array where each element represents the number of new passengers who boarded at each stopover.

Due to regulations, the airline must analyze how many continuous sequences of stopovers (subarrays) had a total of exactly K passengers boarding.

You need to help the airline count the total number of such continuous subarrays where the sum of boarded passengers is equal to the given number K.

Integer array boardedPassengers[] – number of passengers boarded at each stop.
Integer K – the exact sum of passengers required.
Input:
boardedPassengers = {3, 4, 7, 2, -3, 1, 4, 2}
K = 7
Output: 4

    int countSubarraysWithSumK(vector<int>& arr, int K) {
        unordered_map<int, int> prefixSumFreq;
        prefixSumFreq[0] = 1;  // base case for subarrays starting at index 0
    
        int currentSum = 0;
        int count = 0;
    
        for (int i = 0; i < arr.size(); i++) {
            currentSum += arr[i];
    
            // Check if there is a prefix sum that makes currentSum - K
            if (prefixSumFreq.find(currentSum - K) != prefixSumFreq.end()) {
                count += prefixSumFreq[currentSum - K];
            }
    
            // Update frequency of the current prefix sum
            prefixSumFreq[currentSum]++;
        }
    
        return count;
    }
#### OR

    int subarraySum(vector<int>& nums, int k) {
        int presum = 0;
        int count = 0;
        map<int, int> mpp;
        mpp[0] = 1; // handles subarrays starting at index 0
    
        for (int i = 0; i < nums.size(); i++) {
            presum += nums[i];
            int remove = presum - k;
    
            // This line is safe if you use unordered_map or check existence before
            count += mpp[remove]; // if remove doesn't exist, map returns 0
    
            mpp[presum] += 1;
        }
    
        return count;
    }

#### Day 19 - Problem Solving for the day

You are managing a warehouse where boxes are kept in an unordered array of unique labels representing their weights. To improve space optimization, the boxes need to be rearranged in ascending order of weight.

However, due to limited movement, you are allowed to swap only two boxes at a time. Your goal is to determine the minimum number of swaps required to sort the boxes in ascending order of weight.

Constraints:

1.Each box has a unique weight represented as an integer.

2.All weights are positive.

3.You can swap two boxes in one operation.

4.Return the minimum number of swaps needed.

Input:  arr = {4, 3, 2, 1}
Output: 2

    int minimumSwapsToSort(vector<int>& arr) {
        int n = arr.size();
        vector<pair<int, int>> valueIndexPairs(n);
    
        // Pair each value with its original index
        for (int i = 0; i < n; i++) {
            valueIndexPairs[i] = {arr[i], i};
        }
    
        // Sort by value
        sort(valueIndexPairs.begin(), valueIndexPairs.end());
    
        vector<bool> visited(n, false);
        int swaps = 0;
    
        // Detect cycles
        for (int i = 0; i < n; i++) {
            if (visited[i] || valueIndexPairs[i].second == i)
                continue;
    
            int cycle_size = 0;
            int j = i;
    
            while (!visited[j]) {
                visited[j] = true;
                j = valueIndexPairs[j].second; // Go to the next index in the cycle
                cycle_size++;
            }
    
            if (cycle_size > 1)
                swaps += (cycle_size - 1);
        }
    
        return swaps;
    }

#### Day 20 - Problem Solving for the day - 13.06.2025

Next Greater Element (NGE)

You're building a Stock Prediction Tool. For a given array of stock prices for consecutive days, you want to predict the next day with a higher stock price for each day. If no such day exists, the prediction should be -1.

Input:

arr = [4, 5, 2, 25]

Expected Output:

[5, 25, 25, -1]

    vector<int> nextGreaterElements(vector<int>& arr) {
        int n = arr.size();
        vector<int> result(n, -1);
        stack<int> st;  // will store the actual values, not indexes
    
        for (int i = n - 1; i >= 0; i--) {
            // Pop all smaller or equal elements
            while (!st.empty() && st.top() <= arr[i]) {
                st.pop();
            }
    
            if (!st.empty()) {
                result[i] = st.top();
            }
    
            st.push(arr[i]);
        }
    
        return result;
    }

#### Day 22 - Problem Solving for the day - 15.06.2025

Music streaming app 

The app displays a list of songs in a playlist. There’s a special feature where users can “rotate” their playlist — 
meaning, they want to move the last K songs to the beginning of the list to change the listening order.

As a developer, you're tasked with implementing this feature.

Write a Java function that rotates a given integer array (representing song IDs in a playlist) to the right by K positions.

Sample input  : 

songs = [1, 2, 3, 4, 5]

k = 2

Sample output 

[4, 5, 1, 2, 3]

    // Function to reverse a portion of the vector
    void reverse(vector<int>& songs, int start, int end) {
        while (start < end) {
            swap(songs[start], songs[end]);
            start++;
            end--;
        }
    }
    
    // Function to rotate playlist by k positions
    void rotatePlaylist(vector<int>& songs, int k) {
        int n = songs.size();
        k = k % n; // Handle if k > n
    
        // Step 1: Reverse entire playlist
        reverse(songs, 0, n - 1);
        // Step 2: Reverse first k songs
        reverse(songs, 0, k - 1);
        // Step 3: Reverse remaining songs
        reverse(songs, k, n - 1);
    }

#### Day 23 - Problem Solving for the day - 16.06.2025
Given a list of words, group all the anagrams together.
Input :
["listen", "silent", "enlist", "google", "gooegl", "cat", "tac", "act"]

Output :
[[listen, silent, enlist], [google, gooegl], [cat, tac, act]]

    vector<vector<string>> groupAnagrams(vector<string>& strs) {
            unordered_map<string,vector<string>> mpp;
            for(auto str : strs){
                string hashString="";
                vector<int> fre(26,0);
                for(char ch : str){
                    fre[ch-'a']++;
                }
                for(int i=0;i<26;i++){
                    hashString.push_back(fre[i]);
                    hashString.push_back('#');
                }
                mpp[hashString].push_back(str);
            }
            vector<vector<string>> anagram;
            for(auto [key, vectofStrings]: mpp){
                anagram.push_back(vectofStrings);
            }
            return anagram;
        }

#### Day 24 - Problem Solving for the day 

A parking lot in a mall has RxC number of parking spaces. Each parking space will either be  empty(0) or full(1). The status (0/1) of a parking space is represented as the element of the matrix. The task is to find index of the prpeinzta row(R) in the parking lot that has the most of the parking spaces full(1).

Note :

RxC- Size of the matrix
Elements of the matrix M should be only 0 or 1.

Test Case:

Input :

3   -> Value of R(row)
3    -> value of C(column)
[0 1 0 1 1 0 1 1 1] -> Elements of the array M[R][C] where each element is separated by new line.

Output :

3  -> Row 3 has maximum number of 1’s

    int findMaxOnesRow(vector<vector<int>> &matrix) {
        int R = matrix.size();
        int C = matrix[0].size();
        int maxCount = 0;
        int rowIndex = -1;
    
        for (int i = 0; i < R; i++) {
            int count = 0;
            for (int j = 0; j < C; j++) {
                if (matrix[i][j] == 1)
                    count++;
            }
            if (count > maxCount) {
                maxCount = count;
                rowIndex = i + 1; // 1-based indexing
            }
        }
        return rowIndex;
    }

#### Day 25 - Problem Solving for the day 
Given a string str, write a Java program to print all the possible permutations of the characters of the string. The order of the characters in the permutations matters, and each character should be used exactly once in each permutation.

If the string contains duplicate characters, make sure the output includes only unique permutations.

You may print the results in any order.

Test case 1 

input:

str = "abc"

Output:

abc
acb
bac
bca
cab
cba

    #include <iostream>
    #include <vector>
    #include <algorithm>
    
    using namespace std;
    
    void permuteUnique(string &str, vector<bool> &used, string &current, vector<string> &result) {
        if (current.length() == str.length()) {
            result.push_back(current);
            return;
        }
    
        for (int i = 0; i < str.length(); ++i) {
            // Skip already used characters
            if (used[i])
                continue;
    
            // Skip duplicates: if current char is same as previous and previous is not used
            if (i > 0 && str[i] == str[i - 1] && !used[i - 1])
                continue;
    
            used[i] = true;
            current.push_back(str[i]);
            permuteUnique(str, used, current, result);
            current.pop_back();
            used[i] = false;
        }
    }
    
    int main() {
        string str;
        cout << "Enter string: ";
        cin >> str;
    
        sort(str.begin(), str.end()); // Sort to group duplicates
        vector<bool> used(str.length(), false);
        string current;
        vector<string> result;
    
        permuteUnique(str, used, current, result);
    
        for (const string &s : result)
            cout << s << endl;
    
        return 0;
    }

#### Day 26 - Problem Solving for the day

Write a program to count the number of distinct value pairs from a price array such that each pair sums to a given target.
Two pairs (a, b) and (b, a) are considered the same, and each element can be used only once in a pair.
The same value may appear multiple times in the array, but once a value is used in a valid pair, it can't be reused.

Sample Input :

prices = [100, 50, 150, 50, 200]

target = 200

Output :

1

    int countPairsWithTargetSum(vector<int>& prices, int target) {
        sort(prices.begin(), prices.end());
        int left = 0, right = prices.size() - 1;
        int count = 0;
    
        while (left < right) {
            int sum = prices[left] + prices[right];
    
            if (sum == target) {
                count++;
                
                int leftVal = prices[left];
                int rightVal = prices[right];
    
                // Move left to next different element
                while (left < right && prices[left] == leftVal) left++;
                // Move right to previous different element
                while (left < right && prices[right] == rightVal) right--;
            }
            else if (sum < target) {
                left++;
            }
            else {
                right--;
            }
        }
    
        return count;
    }
    
    int main() {
        vector<int> prices = {100, 50, 150, 50, 200};
        int target = 200;
    
        int result = countPairsWithTargetSum(prices, target);
        cout << "Output:\n" << result << endl;
    
        return 0;
    }

#### Day 27 - Problem Solving for the day
A furnishing company is manufacturing a new collection of curtains. The curtains are of two colors aqua(a) and black (b). The curtains color is represented as a string(str) consisting of a’s and b’s of length N. Then, they are packed (substring) into L number of curtains in each box. The box with the maximum number of ‘aqua’ (a) color curtains is labeled. The task here is to find the number of ‘aqua’ color curtains in the labeled box.

Note :

If ‘L’ is not a multiple of N, the remaining number of curtains should be considered as a substring too. In simple words, after dividing the curtains in sets of ‘L’, any curtains left will be another set(refer example 1)

Test Case 1:

Input :

bbbaaababa -> Value of str

3    -> Value of L

Output:

3   -> Maximum number of a’s

    int maxAquaCurtains(string str, int L) {
        int maxCount = 0;
        int n = str.length();
    
        for (int i = 0; i < n; i += L) {
            int count = 0;
    
            // Loop through the substring of length L (or less if at the end)
            for (int j = i; j < i + L && j < n; ++j) {
                if (str[j] == 'a') {
                    count++;
                }
            }
    
            maxCount = max(maxCount, count);
        }
    
        return maxCount;
    }

Day 28 - Problem Solving for the day 

Given a string S(input consisting) of ‘’ and ‘#’. The length of the string is variable. The task is to find the minimum number of ‘’ or ‘#’ to make it a valid string. The string is considered valid if the number of ‘’ and ‘#’ are equal. The ‘’ and ‘#’ can be at any position in the string.
Note : The output will be a positive or negative integer based on number of ‘*’ and ‘#’ in the input string.

(*>#): positive integer
(#>*): negative integer
(#=*): 0
Example 1:
Input 1:

###***   -> Value of S

Output :

0   → number of * and # are equal


    int findBalance(string S) {
        int starCount = 0;
        int hashCount = 0;
    
        for (char ch : S) {
            if (ch == '*') starCount++;
            else if (ch == '#') hashCount++;
        }
    
        return starCount - hashCount; // positive, negative, or 0
    }

Day 29 - Problem Solving for the day - 22.06.2025

Given a string, print non-repeating characters of the string.

Examples:

Example 1:

Input: string = “google”
Output: l,e

Example 2:

Input: string = “yahoo”
Output: y,a,h

    vector<char> getNonRepeatingChars(const string& str) {
        unordered_map<char, int> freq;
        vector<char> result;
    
        // Step 1: Count frequency of each character
        for (char ch : str) {
            freq[ch]++;
        }
    
        // Step 2: Store characters with frequency 1
        for (char ch : str) {
            if (freq[ch] == 1) {
                result.push_back(ch);
            }
        }
    
        return result;
    }

Day 30 - Problem Solving for the day
Problem: Given a String, find the largest word in the string.

Test case 1:
Input: string s=”Google Doc”
Output: “Google”

Test Case 2:
Input: string s=”Microsoft Teams”
Output: “Microsoft”

    int findMaxLengthWordSize(string s) {
        int maxlength = 0;
        string temp = "";
    
        for (int i = 0; i < s.length(); i++) {
            if (s[i] != ' ') {
                temp += s[i];
            } else {
                maxlength = max(maxlength, (int)temp.size());
                temp = ""; // reset for next word
            }
        }
    
        // Check last word if string doesn't end with space
        maxlength = max(maxlength, (int)temp.size());
    
        return maxlength;
    }

Day 32 - Problem Solving for the day 

Write a program to sort characters (numbers and punctuation symbols are not included) in a given string.

Testcase 1:
Input: String str = “zxcbg”
Output: bcgxz

Test case 2:
Input: String str = “edcba”
Output: abcde

    string sortAlphabetsOnly(string str) {
        string letters = "";
    
        // Step 1: Extract only alphabetic characters
        for (int i = 0; i < str.length(); i++) {
            if (isalpha(str[i])) {
                // Optional: convert to lowercase to keep output uniform
                if (str[i] >= 'A' && str[i] <= 'Z')
                    letters += str[i] + 32;
                else
                    letters += str[i];
            }
        }
    
        // Step 2: Bubble Sort on letters
        int n = letters.length();
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (letters[j] > letters[j + 1]) {
                    // Swap
                    char temp = letters[j];
                    letters[j] = letters[j + 1];
                    letters[j + 1] = temp;
                }
            }
        }
    
        return letters;
    }
