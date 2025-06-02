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
