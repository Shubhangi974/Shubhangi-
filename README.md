/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* nums, int numsSize, int target, int* returnSize) {
   int *result=(int*)malloc(2*sizeof(int));
    for(int i=0;i<numsSize;i++){
        for(int j=i+1;j<numsSize;j++){
        if(nums[i]+nums[j]==target){
            result [0]=i;
            result [1]=j;
            *returnSize=2;
            return result;
        }
    }
}
*returnSize=0;
return 0; 
}

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2) {
    struct ListNode* listHead = (struct ListNode*)malloc(sizeof(struct ListNode));
    struct ListNode* listTail = listHead;
    listTail->val = 0;

        while (true) {
        int val = (l1 ? l1->val: 0) + (l2 ? l2->val: 0) + listTail->val;

        listTail->val = val % 10;
        
        l1 = l1 ? l1->next : NULL;
        l2 = l2 ? l2->next : NULL;
        
        if (l1 || l2 || val/10) {
            listTail->next = (struct ListNode*)malloc(sizeof(struct ListNode));
            listTail->next->val = val/10;
            listTail = listTail->next;
            
        }
        else {
            listTail->next = NULL;
            return listHead;
        }
    }
}

int lengthOfLongestSubstring(char* s) {
    if (!s || !*s) // Check if the string is empty
        return 0;
    
    int charIndexMap[256]; // Assuming ASCII characters
    memset(charIndexMap, -1, sizeof(charIndexMap)); // Initialize array with -1
    
    int maxLength = 0;
    int startIndex = 0;
    int i;
    
    for (i = 0; s[i] != '\0'; i++) {
        // If the character is already in the map and its index is after the start index of the current substring,
        // update the start index to the index after the last occurrence of the character
        if (charIndexMap[s[i]] != -1 && charIndexMap[s[i]] >= startIndex) {
            startIndex = charIndexMap[s[i]] + 1;
        }
        
        // Update the index of the current character
        charIndexMap[s[i]] = i;
        
        // Update the maximum length if the current substring is longer
        if (i - startIndex + 1 > maxLength) {
            maxLength = i - startIndex + 1;
        }
    }
    
    return maxLength;
}


int reverse(int x){
    int rev = 0;
    while (x != 0) {
        int pop = x % 10;
        x /= 10;
        if (rev > INT_MAX / 10 || (rev == INT_MAX / 10 && pop > 7)) return 0;
        if (rev < INT_MIN / 10 || (rev == INT_MIN / 10 && pop < -8)) return 0;
        rev = rev * 10 + pop;
    }
    return rev;
}

bool isPalindrome(int x) {
    if(x<0 || x!=0 && x%10 ==0 ) return false;
    int check=0;
    while(x>check){
        check = check*10 + x%10;
        x/=10;
    }
    return (x==check || x==check/10);
}

int romanToInt(char* s) {
    int i=0,ans=0;
    while(s[i]!= '\0')

    {   if(s[i] == 'I' && s[i+1] == 'V'){
           ans = ans +  4; i++;}
        else if(s[i] == 'I' && s[i+1] == 'X'){
           ans = ans +  9; i++;}
        else if(s[i] == 'X' && s[i+1] == 'L'){
           ans = ans +  40; i++;}
        else if(s[i] == 'X' && s[i+1] == 'C'){
           ans = ans +  90; i++; }
        else if(s[i] == 'C' && s[i+1] == 'D'){
           ans = ans +  400; i++;}
        else if(s[i] == 'C' && s[i+1] == 'M'){
           ans = ans +  900; i++;}
        else if(s[i] == 'I')
           ans = ans +  1; 
        else if(s[i] == 'V')
           ans = ans + 5; 
        else if(s[i] == 'X')
           ans = ans +  10;
        else if(s[i] == 'L')
           ans = ans +  50;
        else if(s[i] == 'C')
           ans = ans +  100;
        else if(s[i] == 'D')
           ans = ans +  500;
        else if(s[i] == 'M')
           ans = ans +  1000;  

       i++;      
    }
    return ans;
}

char* longestCommonPrefix(char** strs, int strsSize) {
    char* prefix = (char*)malloc(sizeof(char) * (strlen(strs[0]) + 1));
    int i = -1;

    do{
        i++;
        prefix[i] = strs[0][i];
        for(int j=0; j < strsSize; j++){
            if(!strs[j][i] || prefix[i] != strs[j][i]) 
                prefix[i] = '\0';
        }
    } while(prefix[i]);
    return prefix;
}

bool isValid(char* s) {
    int len =strlen(s);
    char stack[len];
    int top = -1;
    for(int i=0;i<len;i++){
        if(s[i]=='(' || s[i] == '{' || s[i] == '[' ){
            stack[++top] = s[i];
        }else{
            if(top == -1 ||( s[i]==')'&&stack[top] != '(')||(s[i]=='}'&&stack[top] != '{')||(s[i]==']'&&stack[top]!='[')){
                return false;
            } 
            top--;
        } 
    }  
    return top == -1; 
}


/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* mergeTwoLists(struct ListNode* list1, struct ListNode* list2) {
    struct ListNode dummy;
    struct ListNode *current = &dummy;

    while (list1 && list2) {
        if (list1->val <= list2->val) {
            current->next = list1;
            list1 = list1->next;
        } else {
            current->next = list2;
            list2 = list2->next;
        }
        current = current->next;
    }

    current->next = list1 ? list1 : list2;
    return dummy.next;
}

int removeDuplicates(int* nums, int numsSize) {
    if (numsSize == 0) {
        return 0;
    }

    int k = 1; // Initialize the count of unique elements to 1
    for (int i = 1; i < numsSize; i++) {
        if (nums[i] != nums[k - 1]) {
            nums[k] = nums[i]; // Overwrite the next unique element
            k++;
        }
    }

    return k;
}

int removeDuplicates(int* nums, int numsSize) {
    if (numsSize == 0) {
        return 0;
    }

    int k = 1; // Initialize the count of unique elements to 1
    for (int i = 1; i < numsSize; i++) {
        if (nums[i] != nums[k - 1]) {
            nums[k] = nums[i]; // Overwrite the next unique element
            k++;
        }
    }

    return k;
}

int removeElement(int* nums, int numsSize, int val) {
    int count = 0;

    for (int i = 0; i < numsSize; i++)
        if (nums[i] == val) 
            count++;
        else 
            nums[i - count] = nums[i];
    return (numsSize - count);
}

int strStr(char* haystack, char* needle) {
    int haystack_size = strlen(haystack);
    int needle_size = strlen(needle);
    int result = -1;
    int i = 0;  // haystack
    int j = 0;  // needle

    while (i < (haystack_size) && j < needle_size) {
        if (haystack[i] == needle[j]) {
            i++;
            j++;
        }
        else {
            i = i - j + 1;
            j = 0;
        }
    }

    return result = (j == needle_size) ? (i - needle_size) : -1;
}

int searchInsert(int* nums, int numsSize, int target) {
    for(int i = 0; i < numsSize; i++){
        if(nums[i] >= target)   return i;
    }
    return numsSize;
}

int lengthOfLastWord(char* s) {
   char *token;
    token=strtok(s, " ");
    int len;
    do{
        len=strlen(token);
    //    printf("%s\n", token);
    }while (token=strtok(NULL, " "));

    return len; 
}

/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* plusOne(int* digits, int digitsSize, int* returnSize) {
    *returnSize = digitsSize;
    int *plusOne = malloc(digitsSize * sizeof(int));
    if (plusOne == NULL)
        return (NULL);
    for (int i = 0; i < digitsSize; i++)
        plusOne[i] = digits[i];
    
    plusOne[digitsSize - 1]++;
    for (int i = digitsSize - 1; i - 1 >= 0; i--)
        if (plusOne[i] == 10) {
            plusOne[i] = 0;
            plusOne[i - 1]++;
        }

    if (plusOne[0] == 10) {
        (*returnSize)++;
        plusOne = realloc(plusOne, *returnSize * sizeof(int));
        if (plusOne == NULL)
            return (NULL);
        memmove(plusOne + 1, plusOne, digitsSize * sizeof(int));
        plusOne[0] = 1;
        plusOne[1] = 0;
    }
    return (plusOne);
}

int mySqrt(int x) {
    /*Naive approach
    int i = 0;
    while(i*i <= x) i++;
    return i - 1;*/

    long long int low = 1, high = x, mid;
    while(low <= high){
        mid = low +(high - low) / 2;
        if(mid*mid > x)
            high = mid - 1;
        else
            low = mid + 1;
    }

    return high;
}

int climbStairs(int n) {
    long long int prv1 = 1;
    long long int prv2 = 1;

    for (int i = 0; i < n; i++) {
        long long int tmp = prv1;
        prv1 = prv1 + prv2;
        prv2 = tmp;
    }

    return (int)prv2;
}

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* deleteDuplicates(struct ListNode* head) {
    struct ListNode* h1=head;
        while(h1 != NULL && h1->next != NULL){
            if(h1->val==h1->next->val){
                h1->next=h1->next->next;
            }else{
                h1=h1->next;
            }
        }
        return head;
}

void merge(int* nums1, int nums1Size, int m, int* nums2, int nums2Size, int n) {
    int i = m - 1;
    int j = n -1;
    // Create a loop until either of i or j becomes zero...
    while(i>=0 && j>=0) {
        if(nums1[i] > nums2[j]) {
            nums1[i+j+1] = nums1[i];
            i--;
        } else {
            nums1[i+j+1] = nums2[j];
            j--;
        }
    }
    // While j does not become zero...
    while(j >= 0) {
        nums1[i+j+1] = nums2[j];
        j--;
    }
}

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
bool isSameTree(struct TreeNode* p, struct TreeNode* q) {
    if (p == NULL && q == NULL) {
        return true;
    } else if (p == NULL || q == NULL) {
        return false;
    } else {
        return p->val == q->val && isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
}

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int maxDepth(struct TreeNode* root) {
     if (!root)
        return 0;
    int left_depth = maxDepth(root->left);
    int right_depth = maxDepth(root->right);
    if(left_depth > right_depth)
        return left_depth +1;
    else
        return right_depth +1;
}

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
struct TreeNode* sortedArrayToBST(int* nums, int numsSize) {
    struct TreeNode* t;
    if (numsSize == 0) {
        return 0;
    }
    int mid = numsSize / 2;
    t = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    t->val = nums[mid];
    t->left=sortedArrayToBST(nums,mid);
    t->right=sortedArrayToBST(nums+mid+1,numsSize-mid-1);
    return t;
}

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
struct TreeNode* sortedArrayToBST(int* nums, int numsSize) {
    struct TreeNode* t;
    if (numsSize == 0) {
        return 0;
    }
    int mid = numsSize / 2;
    t = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    t->val = nums[mid];
    t->left=sortedArrayToBST(nums,mid);
    t->right=sortedArrayToBST(nums+mid+1,numsSize-mid-1);
    return t;
}

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int minDepth(struct TreeNode* root) {
  if(root==NULL)  return 0;
    if(root->left==NULL)    return 1+minDepth(root->right);
    if(root->right==NULL)   return 1+minDepth(root->left);

    int leftheight = minDepth(root->left)+1;
    int rightheight = minDepth(root->right)+1;

    return (leftheight<rightheight)?leftheight:rightheight;
      
}

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
bool hasPathSum(struct TreeNode* root, int targetSum) {
    if (root == NULL) return false;
    // If there is only a single root node and the value of root node is equal to the targetSum...
	if (root->val == targetSum && (root->left == NULL && root->right == NULL)) return true;
    // Call the same function recursively for left and right subtree...
	return hasPathSum(root->left, targetSum - root->val)|| hasPathSum(root->right, targetSum - root->val);
}

/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *returnColumnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** generate(int numRows, int* returnSize, int** returnColumnSizes) {
    int** a = (int**)malloc(sizeof(int*) * numRows);
    *returnSize = numRows;
    *returnColumnSizes=(int*)malloc(sizeof(int)*numRows); //Allocate for column sizes array

    for (register int i=0; i<numRows; i++) {
        (*returnColumnSizes)[i] =i+1; // Set the size of each row in column sizes array
        a[i] = (int*)malloc(sizeof(int)*(i+1));

        a[i][0] = a[i][i] = 1;
        for (register int j=1; j<=i/2; j++) {
            a[i][j]=a[i][i-j]= a[i-1][j-1]+a[i-1][j];
        }
    }
    return a;
}
