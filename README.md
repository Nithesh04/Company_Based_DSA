# Company_Based_DSA
##Q1: You are working as a software developer for financial analytics firms. One of your models receives an array of daily stock performance indicators. Positive numbers indicate profitable days, negative numbers indicate lost days. To improve the model, one should alternate profit and loss days in the report. If there are more positive or more negative days, the excess should be placed at the end in the same order. write a c++ function to rearrange the given array so that positive and negative numbers appear alternatively. the relative order of elements should be preserved as much as possible. testcase1: 5,-1,3,2,-7,8,9   output: 5,-1,3,-7,2,8,9 test case 2: -3,-2,4,-1,-6,7  output: -3,4,-2,7,-1,-6

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
