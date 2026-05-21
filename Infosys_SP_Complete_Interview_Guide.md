#Interview Guide (Expanded)

A full coverage guide: process, **40+ coding problems with C++ solutions**, OOPs, DBMS, OS, CN, SQL, Java/Python theory, aptitude, puzzles, and HR — based on real interview experiences.

---

## 1. Hiring Pipeline

| Stage | Format | Duration |
|---|---|---|
| **InfyTQ Certification** (eligibility) | 20 MCQs (Java/Python + DBMS) + 2 coding | ~2 hrs |
| **HackWithInfy / Coding Round** | 3 problems: easy + medium + hard | 3 hrs |
| **Round 2 Coding** (sometimes) | 2 problems offline at DC | 2 hrs |
| **Technical Interview** | DSA on paper + OOPs + DBMS + OS + CN + project + SQL queries | 45–60 min |
| **HR Round** | Behavioral, fit, location | 15–20 min |

**Topics tested in coding:** arrays, strings, prefix sum, hashing, two pointers, sliding window, recursion, backtracking, greedy, DP (0/1 knapsack, LCS, MCM, DP on trees), graphs (BFS/DFS, Dijkstra, topological sort), trees, linked lists, heap, trie, bit manipulation, math (GCD, sieve, modular arithmetic).

**Cut-off mindset:** Aim for 2 of 3 fully correct. Submit brute-force partials — partial scoring exists.

---

## 2. Coding Problems with C++ Solutions

### A. Arrays & Strings


**1. Reverse a string**
```cpp
string reverseString(string s) {
    int i = 0, j = s.size() - 1;
    while (i < j) swap(s[i++], s[j--]);
    return s;
}
```

**2. Check palindrome**
```cpp
bool isPalindrome(string s) {
    int i = 0, j = s.size() - 1;
    while (i < j) if (s[i++] != s[j--]) return false;
    return true;
}
```


**3. Next palindrome number**
```cpp
bool isPal(long long n){ string s=to_string(n); int i=0,j=s.size()-1;
    while(i<j) if(s[i++]!=s[j--]) return false; return true; }
long long nextPalindrome(long long n){ n++; while(!isPal(n)) n++; return n; }
```

**4. First non-repeating character**
```cpp
char firstUnique(string s) {
    int freq[26] = {0};
    for (char c : s) freq[c - 'a']++;
    for (char c : s) if (freq[c - 'a'] == 1) return c;
    return '_';
}
```

**5. Check anagram**
```cpp
bool isAnagram(string a, string b) {
    if (a.size() != b.size()) return false;
    int cnt[26] = {0};
    for (char c : a) cnt[c - 'a']++;
    for (char c : b) if (--cnt[c - 'a'] < 0) return false;
    return true;
}
```

**6. Reverse words in a string**
```cpp
string reverseWords(string s) {
    stringstream ss(s);
    string word, result;
    vector<string> words;
    while (ss >> word) words.push_back(word);
    for (int i = words.size() - 1; i >= 0; i--)
        result += words[i] + (i > 0 ? " " : "");
    return result;
}
```

**7. Longest substring without repeating characters (sliding window)  --**
```cpp
int longestUnique(string s) {
    unordered_map<char, int> last;
    int start = 0, maxLen = 0;
    for (int i = 0; i < s.size(); i++) {
        if (last.count(s[i]) && last[s[i]] >= start)
            start = last[s[i]] + 1;
        last[s[i]] = i;
        maxLen = max(maxLen, i - start + 1);
    }
    return maxLen;
}
```

**8. Two sum**
```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> mp;
    for (int i = 0; i < nums.size(); i++) {
        if (mp.count(target - nums[i])) return {mp[target - nums[i]], i};
        mp[nums[i]] = i;
    }
    return {};
}
```


**9. Three sum (find all triplets summing to 0)**
```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    int n = nums.size();
    for (int i = 0; i < n - 2; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        int l = i + 1, r = n - 1;
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum == 0) {
                res.push_back({nums[i], nums[l], nums[r]});
                while (l < r && nums[l] == nums[l+1]) l++;
                while (l < r && nums[r] == nums[r-1]) r--;
                l++; r--;
            } else if (sum < 0) l++;
            else r--;
        }
    }
    return res;
}
```

**10. Sort 0s, 1s, 2s — Dutch flag**
```cpp
void sort012(vector<int>& a) {
    int low = 0, mid = 0, high = a.size() - 1;
    while (mid <= high) {
        if (a[mid] == 0) swap(a[low++], a[mid++]);
        else if (a[mid] == 1) mid++;
        else swap(a[mid], a[high--]);
    }
}
```

**11. Maximum subarray sum — Kadane's**
```cpp
int maxSubArray(vector<int>& a) {
    int curr = a[0], best = a[0];
    for (int i = 1; i < a.size(); i++) {
        curr = max(a[i], curr + a[i]);
        best = max(best, curr);
    }
    return best;
}
```

**12. Stock buy/sell — max profit (one transaction)**
```cpp
int maxProfit(vector<int>& prices) {
    int minP = INT_MAX, profit = 0;
    for (int p : prices) {
        minP = min(minP, p);
        profit = max(profit, p - minP);
    }
    return profit;
}
```

**13. Rotate array by k**
```cpp
void rotate(vector<int>& a, int k) {
    int n = a.size(); k %= n;
    reverse(a.begin(), a.end());
    reverse(a.begin(), a.begin() + k);
    reverse(a.begin() + k, a.end());
}
```

**14. Find missing number 1..n**
```cpp
int missingNumber(vector<int>& a, int n) {
    long long total = (long long)n * (n + 1) / 2;
    long long sum = 0;
    for (int x : a) sum += x;
    return total - sum;
}
```

**15. Find duplicate (Floyd's tortoise–hare)**
```cpp
int findDuplicate(vector<int>& nums) {
    int slow = nums[0], fast = nums[0];
    do { slow = nums[slow]; fast = nums[nums[fast]]; } while (slow != fast);
    slow = nums[0];
    while (slow != fast) { slow = nums[slow]; fast = nums[fast]; }
    return slow;
}
```

**16. Merge two sorted arrays in place (no extra space)**
```cpp
void merge(vector<int>& a, vector<int>& b) {
    int n = a.size(), m = b.size();
    int gap = (n + m + 1) / 2;
    while (gap > 0) {
        int i = 0, j = gap;
        while (j < n + m) {
            int x = (i < n) ? a[i] : b[i - n];
            int y = (j < n) ? a[j] : b[j - n];
            if (x > y) {
                if (i < n && j < n) swap(a[i], a[j]);
                else if (i < n) swap(a[i], b[j - n]);
                else swap(b[i - n], b[j - n]);
            }
            i++; j++;
        }
        gap = (gap == 1) ? 0 : (gap + 1) / 2;
    }
}
```

### B. Linked Lists

```cpp
struct Node { int data; Node* next; };
```

**17. Reverse a linked list done **
```cpp
Node* reverse(Node* head) {
    Node *prev = NULL, *curr = head;
    while (curr) {
        Node* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

**18. Detect cycle (Floyd's) -- done**
```cpp
bool hasCycle(Node* head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

**19. Find middle **
```cpp
Node* middle(Node* head) {
    Node *slow = head, *fast = head;
    while (fast && fast->next) { slow = slow->next; fast = fast->next->next; }
    return slow;
}
```

**20. Merge two sorted linked lists**
```cpp
Node* mergeTwo(Node* a, Node* b) {
    Node dummy(0), *tail = &dummy;
    while (a && b) {
        if (a->data <= b->data) { tail->next = a; a = a->next; }
        else { tail->next = b; b = b->next; }
        tail = tail->next;
    }
    tail->next = a ? a : b;
    return dummy.next;
}
```

**21. Remove Nth node from end**
```cpp
Node* removeNth(Node* head, int n) {
    Node dummy(0); dummy.next = head;
    Node *fast = &dummy, *slow = &dummy;
    for (int i = 0; i < n; i++) fast = fast->next;
    while (fast->next) { fast = fast->next; slow = slow->next; }
    slow->next = slow->next->next;
    return dummy.next;
}
```

### C. Stacks & Queues

**22. Valid parentheses**
```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') st.push(c);
        else {
            if (st.empty()) return false;
            char top = st.top(); st.pop();
            if ((c == ')' && top != '(') || (c == '}' && top != '{') ||
                (c == ']' && top != '[')) return false;
        }
    }
    return st.empty();
}
```

**23. Next greater element**
```cpp
vector<int> nextGreater(vector<int>& a) {
    int n = a.size();
    vector<int> res(n, -1);
    stack<int> st;
    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && st.top() <= a[i]) st.pop();
        if (!st.empty()) res[i] = st.top();
        st.push(a[i]);
    }
    return res;
}
```

**24. Implement queue using two stacks**
```cpp
class MyQueue {
    stack<int> in, out;
public:
    void push(int x) { in.push(x); }
    int pop() { peek(); int x = out.top(); out.pop(); return x; }
    int peek() {
        if (out.empty()) while (!in.empty()) { out.push(in.top()); in.pop(); }
        return out.top();
    }
    bool empty() { return in.empty() && out.empty(); }
};
```

### D. Trees

```cpp
struct TreeNode { int val; TreeNode *left, *right; };
```

**25. Inorder/Preorder/Postorder**
```cpp
void inorder(TreeNode* r){ if(!r) return; inorder(r->left); cout<<r->val<<" "; inorder(r->right); }
void preorder(TreeNode* r){ if(!r) return; cout<<r->val<<" "; preorder(r->left); preorder(r->right); }
void postorder(TreeNode* r){ if(!r) return; postorder(r->left); postorder(r->right); cout<<r->val<<" "; }
```

**26. Level order (BFS)**
```cpp
void levelOrder(TreeNode* root) {
    if (!root) return;
    queue<TreeNode*> q; q.push(root);
    while (!q.empty()) {
        TreeNode* n = q.front(); q.pop();
        cout << n->val << " ";
        if (n->left) q.push(n->left);
        if (n->right) q.push(n->right);
    }
}
```

**27. Height of binary tree**
```cpp
int height(TreeNode* r) {
    if (!r) return 0;
    return 1 + max(height(r->left), height(r->right));
}
```

**28. Lowest Common Ancestor (BST)**
```cpp
TreeNode* lcaBST(TreeNode* root, int p, int q) {
    while (root) {
        if (p < root->val && q < root->val) root = root->left;
        else if (p > root->val && q > root->val) root = root->right;
        else return root;
    }
    return NULL;
}
```

**29. Check if tree is balanced**
```cpp
int check(TreeNode* r) {
    if (!r) return 0;
    int l = check(r->left); if (l == -1) return -1;
    int rt = check(r->right); if (rt == -1) return -1;
    if (abs(l - rt) > 1) return -1;
    return 1 + max(l, rt);
}
bool isBalanced(TreeNode* r) { return check(r) != -1; }
```

**30. Diameter of binary tree**
```cpp
int diameter = 0;
int depth(TreeNode* r) {
    if (!r) return 0;
    int l = depth(r->left), rt = depth(r->right);
    diameter = max(diameter, l + rt);
    return 1 + max(l, rt);
}
```

### E. Graphs

**31. BFS traversal**
```cpp
void bfs(int start, vector<vector<int>>& adj, int n) {
    vector<bool> vis(n, false);
    queue<int> q; q.push(start); vis[start] = true;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        cout << u << " ";
        for (int v : adj[u]) if (!vis[v]) { vis[v] = true; q.push(v); }
    }
}
```

**32. DFS traversal**
```cpp
void dfs(int u, vector<vector<int>>& adj, vector<bool>& vis) {
    vis[u] = true; cout << u << " ";
    for (int v : adj[u]) if (!vis[v]) dfs(v, adj, vis);
}
```

**33. Detect cycle in undirected graph**
```cpp
bool dfsCycle(int u, int parent, vector<vector<int>>& adj, vector<bool>& vis) {
    vis[u] = true;
    for (int v : adj[u]) {
        if (!vis[v]) { if (dfsCycle(v, u, adj, vis)) return true; }
        else if (v != parent) return true;
    }
    return false;
}
```

**34. Topological sort (Kahn's algorithm)**
```cpp
vector<int> topoSort(int n, vector<vector<int>>& adj) {
    vector<int> indeg(n, 0);
    for (int u = 0; u < n; u++) for (int v : adj[u]) indeg[v]++;
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    vector<int> order;
    while (!q.empty()) {
        int u = q.front(); q.pop(); order.push_back(u);
        for (int v : adj[u]) if (--indeg[v] == 0) q.push(v);
    }
    return order;
}
```

**35. Dijkstra's shortest path**
```cpp
vector<int> dijkstra(int src, int n, vector<vector<pair<int,int>>>& adj) {
    vector<int> dist(n, INT_MAX); dist[src] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

### F. Dynamic Programming

**36. Fibonacci (memoization)**
```cpp
int memo[1001];
int fib(int n) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    return memo[n] = fib(n-1) + fib(n-2);
}
```

**37. Climb stairs**
```cpp
int climbStairs(int n) {
    if (n <= 2) return n;
    int a = 1, b = 2;
    for (int i = 3; i <= n; i++) { int c = a + b; a = b; b = c; }
    return b;
}
```

**38. Longest Common Subsequence**
```cpp
int lcs(string a, string b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = (a[i-1]==b[j-1]) ? dp[i-1][j-1]+1 : max(dp[i-1][j], dp[i][j-1]);
    return dp[m][n];
}
```

**39. Longest Increasing Subsequence — O(n log n)**
```cpp
int lis(vector<int>& a) {
    vector<int> tails;
    for (int x : a) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}
```

**40. 0/1 Knapsack**
```cpp
int knapsack(int W, vector<int>& wt, vector<int>& val, int n) {
    vector<vector<int>> dp(n+1, vector<int>(W+1, 0));
    for (int i = 1; i <= n; i++)
        for (int w = 0; w <= W; w++) {
            dp[i][w] = dp[i-1][w];
            if (wt[i-1] <= w)
                dp[i][w] = max(dp[i][w], dp[i-1][w-wt[i-1]] + val[i-1]);
        }
    return dp[n][W];
}
```

**41. Coin change (minimum coins)**
```cpp
int coinChange(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, amount + 1);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++)
        for (int c : coins) if (c <= i) dp[i] = min(dp[i], dp[i-c] + 1);
    return dp[amount] > amount ? -1 : dp[amount];
}
```

**42. Edit distance**
```cpp
int editDistance(string a, string b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1));
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = (a[i-1]==b[j-1]) ? dp[i-1][j-1]
                       : 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
    return dp[m][n];
}
```

**43. Matrix chain multiplication**
```cpp
int mcm(vector<int>& p) {
    int n = p.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int len = 2; len < n; len++)
        for (int i = 1; i < n - len + 1; i++) {
            int j = i + len - 1;
            dp[i][j] = INT_MAX;
            for (int k = i; k < j; k++)
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j]);
        }
    return dp[1][n-1];
}
```

### G. Searching & Sorting

**44. Binary search**
```cpp
int binarySearch(vector<int>& a, int target) {
    int l = 0, r = a.size() - 1;
    while (l <= r) {
        int m = l + (r - l) / 2;
        if (a[m] == target) return m;
        if (a[m] < target) l = m + 1; else r = m - 1;
    }
    return -1;
}
```

**45. Quick sort**
```cpp
int partition(vector<int>& a, int l, int r) {
    int pivot = a[r], i = l - 1;
    for (int j = l; j < r; j++) if (a[j] < pivot) swap(a[++i], a[j]);
    swap(a[i+1], a[r]);
    return i + 1;
}
void quickSort(vector<int>& a, int l, int r) {
    if (l < r) { int p = partition(a, l, r); quickSort(a, l, p-1); quickSort(a, p+1, r); }
}
```

**46. Merge sort**
```cpp
void merge(vector<int>& a, int l, int m, int r) {
    vector<int> tmp(r - l + 1);
    int i = l, j = m + 1, k = 0;
    while (i <= m && j <= r) tmp[k++] = (a[i] <= a[j]) ? a[i++] : a[j++];
    while (i <= m) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];
    for (int x = 0; x < tmp.size(); x++) a[l + x] = tmp[x];
}
void mergeSort(vector<int>& a, int l, int r) {
    if (l < r) { int m = (l + r) / 2; mergeSort(a, l, m); mergeSort(a, m+1, r); merge(a, l, m, r); }
}
```

### H. Math & Bit Manipulation

**47. Check prime — O(√n)**
```cpp
bool isPrime(int n) {
    if (n < 2) return false;
    if (n % 2 == 0) return n == 2;
    for (int i = 3; (long long)i * i <= n; i += 2) if (n % i == 0) return false;
    return true;
}
```

**48. Sieve of Eratosthenes**
```cpp
vector<bool> sieve(int n) {
    vector<bool> p(n+1, true); p[0] = p[1] = false;
    for (int i = 2; (long long)i * i <= n; i++)
        if (p[i]) for (int j = i*i; j <= n; j += i) p[j] = false;
    return p;
}
```

**49. GCD (Euclidean)**
```cpp
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
```

**50. Fast power — a^b mod m**
```cpp
long long power(long long a, long long b, long long m) {
    long long res = 1; a %= m;
    while (b > 0) {
        if (b & 1) res = res * a % m;
        a = a * a % m;
        b >>= 1;
    }
    return res;
}
```

**51. Count set bits**
```cpp
int countBits(int n) {
    int c = 0;
    while (n) { n &= (n - 1); c++; }
    return c;
}
```

**52. Find single non-repeating element (XOR)**
```cpp
int singleNumber(vector<int>& a) {
    int x = 0;
    for (int v : a) x ^= v;
    return x;
}
```

**53. Reverse bits of an integer**
```cpp
unsigned int reverseBits(unsigned int n) {
    unsigned int res = 0;
    for (int i = 0; i < 32; i++) {
        res = (res << 1) | (n & 1);
        n >>= 1;
    }
    return res;
}
```

### I. Backtracking

**54. Permutations**
```cpp
void permute(vector<int>& a, int idx, vector<vector<int>>& res) {
    if (idx == a.size()) { res.push_back(a); return; }
    for (int i = idx; i < a.size(); i++) {
        swap(a[idx], a[i]);
        permute(a, idx + 1, res);
        swap(a[idx], a[i]);
    }
}
```

**55. N-Queens**
```cpp
bool safe(vector<string>& b, int r, int c, int n) {
    for (int i = 0; i < r; i++) if (b[i][c] == 'Q') return false;
    for (int i = r-1, j = c-1; i>=0 && j>=0; i--, j--) if (b[i][j]=='Q') return false;
    for (int i = r-1, j = c+1; i>=0 && j<n; i--, j++) if (b[i][j]=='Q') return false;
    return true;
}
void solveNQ(int r, int n, vector<string>& b, vector<vector<string>>& res) {
    if (r == n) { res.push_back(b); return; }
    for (int c = 0; c < n; c++) {
        if (safe(b, r, c, n)) {
            b[r][c] = 'Q';
            solveNQ(r + 1, n, b, res);
            b[r][c] = '.';
        }
    }
}
```

---

## 3. OOPs — Theory

**The 4 pillars:**
- **Encapsulation** — bundling data + methods, restricting direct access via private members.
- **Inheritance** — child class derives from parent (code reuse, IS-A).
- **Polymorphism** — same interface, different forms (overloading at compile time, overriding at runtime).
- **Abstraction** — exposing only what matters via abstract classes / interfaces.

**Common Q&A:**
- **Encapsulation vs Abstraction** — Encapsulation hides *data*; Abstraction hides *implementation*.
- **Overloading vs Overriding** — Overloading: same class, different params, compile-time. Overriding: child redefines parent's method, runtime.
- **Types of inheritance** — Single, Multiple, Multilevel, Hierarchical, Hybrid. (Java doesn't allow multiple inheritance via classes — only via interfaces.)
- **Abstract class vs Interface** — Abstract: can have implemented methods + state, single inheritance. Interface: pure contract, multiple inheritance.
- **Virtual function** — method declared with `virtual` enabling runtime polymorphism via vtable.
- **Pure virtual function** — `virtual void f() = 0;` makes class abstract.
- **Constructor vs Destructor** — Constructor initializes; destructor `~ClassName()` releases resources.
- **Copy constructor** — `Class(const Class& other)` — creates a deep/shallow copy.
- **Static vs non-static** — Static: belongs to class, shared. Non-static: per object.
- **`this` pointer** — implicit pointer to current object in non-static methods.
- **Friend function** — non-member function with access to private members.
- **Operator overloading** — redefining operators for user-defined types in C++.

---

## 4. DBMS — Theory

- **DBMS vs RDBMS** — RDBMS stores data in tables with relations; DBMS is broader.
- **Normalization** — reduces redundancy.
  - **1NF** — atomic values
  - **2NF** — 1NF + no partial dependency
  - **3NF** — 2NF + no transitive dependency
  - **BCNF** — every determinant is a candidate key
- **Denormalization** — adding redundancy for performance.
- **ACID** — Atomicity, Consistency, Isolation, Durability.
- **Keys** — Primary, Candidate, Foreign, Super, Composite, Unique.
- **Primary vs Unique** — Primary: NOT NULL + only one. Unique: one NULL allowed + multiple.
- **Indexes** — speed up SELECT (B-tree/hash); slow down INSERT/UPDATE.
- **Joins** — INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF.
- **Transaction states** — Active, Partially committed, Committed, Failed, Aborted.
- **Concurrency control** — Locks, timestamps, MVCC.
- **Deadlock prevention** — Wait-die, Wound-wait schemes.
- **Trigger** — auto-executed procedure on INSERT/UPDATE/DELETE.
- **View** — virtual table from a query; stored as definition, not data.
- **Stored procedure** — precompiled SQL routine.

---

## 5. SQL Queries (Frequently Asked)

**Schema:** `Employee(id, name, salary, dept_id)`, `Dept(dept_id, dept_name)`

```sql
-- 1. 2nd highest salary
SELECT MAX(salary) FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);

-- 2. Nth highest salary using DENSE_RANK
SELECT salary FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) rk
  FROM Employee
) t WHERE rk = N;

-- 3. Find duplicates
SELECT name, COUNT(*) FROM Employee
GROUP BY name HAVING COUNT(*) > 1;

-- 4. Delete duplicates (keep one)
DELETE e1 FROM Employee e1
INNER JOIN Employee e2
WHERE e1.id > e2.id AND e1.name = e2.name;

-- 5. Department-wise highest salary
SELECT dept_id, MAX(salary) FROM Employee GROUP BY dept_id;

-- 6. Employees earning more than dept average
SELECT e.* FROM Employee e
WHERE salary > (SELECT AVG(salary) FROM Employee WHERE dept_id = e.dept_id);

-- 7. Join with department name
SELECT e.name, d.dept_name FROM Employee e
INNER JOIN Dept d ON e.dept_id = d.dept_id;

-- 8. Employees with no manager (self-join)
SELECT e.name FROM Employee e
LEFT JOIN Employee m ON e.manager_id = m.id
WHERE m.id IS NULL;

-- 9. Top 3 salaries per department
SELECT * FROM (
  SELECT *, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) rk
  FROM Employee
) t WHERE rk <= 3;

-- 10. Count employees per department
SELECT dept_id, COUNT(*) FROM Employee GROUP BY dept_id;
```

**WHERE vs HAVING:** WHERE filters rows pre-aggregation; HAVING filters groups post-aggregation.
**UNION vs UNION ALL:** UNION removes duplicates (slower); UNION ALL keeps everything.

---

## 6. Operating Systems

- **Process vs Thread** — Process: independent, own memory. Thread: lightweight, shared memory within a process.
- **Process states** — New, Ready, Running, Waiting, Terminated.
- **Context switch** — saving/restoring CPU state when switching processes.
- **Deadlock** — 4 conditions: Mutual Exclusion, Hold and Wait, No Preemption, Circular Wait.
- **Deadlock handling** — Prevention, Avoidance (Banker's), Detection + Recovery, Ignore.
- **Scheduling algorithms** — FCFS, SJF, SRTF, Priority, Round Robin, Multilevel Queue.
- **Paging vs Segmentation** — Paging: fixed pages, no external fragmentation. Segmentation: variable, logical units.
- **Virtual memory** — illusion of contiguous large memory using disk swap.
- **Page replacement** — FIFO, LRU, Optimal, Clock.
- **Thrashing** — excessive paging, low CPU utilization.
- **Mutex vs Semaphore** — Mutex: binary, owner-released. Semaphore: counter, any thread can signal.
- **Critical section problem** — solutions: Peterson's, semaphores, monitors.
- **Producer-consumer problem** — classic synchronization using semaphores.
- **Reader-writer problem** — multiple readers OK, exclusive writer.
- **System calls** — fork(), exec(), wait(), exit(), read(), write().
- **fork()** — creates child process; returns 0 to child, child PID to parent.
- **User mode vs Kernel mode** — restricted vs full access; switch via system calls.

---

## 7. Computer Networks

- **OSI Model** — Physical, Data Link, Network, Transport, Session, Presentation, Application. *(Please Do Not Throw Sausage Pizza Away)*
- **TCP/IP Model** — Link, Internet, Transport, Application.
- **TCP vs UDP** — TCP: reliable, ordered, slower. UDP: unreliable, faster, used in DNS, video streaming.
- **3-way handshake** — SYN → SYN-ACK → ACK.
- **4-way termination** — FIN → ACK → FIN → ACK.
- **HTTP vs HTTPS** — HTTPS = HTTP + TLS, encrypted, port 443.
- **GET vs POST** — GET in URL (cacheable, idempotent); POST in body.
- **Status codes** — 200 OK, 301 Moved, 400 Bad Req, 401 Unauth, 404 Not Found, 500 Server Error.
- **DNS** — translates domain → IP using hierarchy (root → TLD → authoritative).
- **What happens when you type a URL?** — DNS lookup → TCP handshake → TLS → HTTP request → response → render.
- **IPv4 vs IPv6** — 32-bit vs 128-bit; IPv6 has built-in security and larger address space.
- **MAC vs IP** — MAC: hardware address, layer 2. IP: logical address, layer 3.
- **Subnet mask** — partitions IP into network + host portions.
- **NAT** — Network Address Translation; maps private to public IPs.
- **Routers vs Switches vs Hubs** — Router: layer 3. Switch: layer 2. Hub: layer 1, broadcasts to all.
- **ARP** — resolves IP → MAC. **RARP** — MAC → IP.
- **Firewall** — filters packets based on rules.

---

## 8. Java / Python Theory (per language chosen in InfyTQ)

### Java
- **JDK vs JRE vs JVM** — JDK: development kit. JRE: runtime. JVM: executes bytecode.
- **String vs StringBuilder vs StringBuffer** — String: immutable. StringBuilder: mutable, not thread-safe. StringBuffer: mutable, thread-safe.
- **`==` vs `.equals()`** — `==` compares references; `.equals()` compares content.
- **HashMap vs HashTable** — HashMap: not synchronized, allows null. HashTable: synchronized, no null.
- **ArrayList vs LinkedList** — ArrayList: O(1) random access. LinkedList: O(1) insert/delete at known position.
- **Final, Finally, Finalize** — final: constant/can't override. finally: always-run block. finalize(): GC callback (deprecated).
- **Checked vs Unchecked exceptions** — Checked: must handle (IOException). Unchecked: runtime (NullPointerException).
- **Abstract class vs Interface** — Abstract has state + concrete methods; interface (Java 8+) can have default methods.
- **Multithreading** — `Thread` class or `Runnable` interface; `synchronized` keyword for safety.
- **Garbage collection** — automatic memory management; uses generations (young, old).

### Python
- **List vs Tuple** — List mutable, tuple immutable.
- **Dict vs Set** — Dict: key-value. Set: unique values.
- **`is` vs `==`** — `is` checks identity (same object); `==` checks equality.
- **`*args` and `**kwargs`** — variable positional / keyword args.
- **List comprehension** — `[x*x for x in range(10) if x%2==0]`.
- **Decorators** — wrap functions to add behavior (e.g., `@staticmethod`).
- **Generators** — lazy iterators using `yield`.
- **GIL** — Global Interpreter Lock; only one thread executes Python bytecode at a time in CPython.
- **PEP 8** — style guide.
- **Mutable default args pitfall** — `def f(x=[])` — list shared across calls; use `None` instead.

---

## 9. Aptitude (sample asked)

- **Number systems** — binary/decimal/hex conversions.
- **HCF/LCM** — `HCF * LCM = a * b`.
- **Time/work** — A does work in *a* days, B in *b* days → together: `1/(1/a + 1/b)` days.
- **Trains** — relative speed (same direction: subtract; opposite: add).
- **Permutation/Combination** — `nPr = n!/(n-r)!`, `nCr = n!/(r!(n-r)!)`.
- **Probability** — favorable / total.
- **Profit/Loss** — `% = (SP-CP)/CP * 100`.

---

## 10. Logical Puzzles (sometimes asked)

- **25 horses, 5 tracks** → minimum races to find top 3 = **7**.
- **3 bulbs, 3 switches, 1 visit** → use heat (turn one ON for a while, switch off; turn second ON; visit: ON = 2nd, OFF+warm = 1st, OFF+cold = 3rd).
- **8 balls, 1 heavier, 2 weighings** → divide 3-3-2; weigh first 3 vs second 3.
- **Bridge crossing** — 4 people, 1 torch, fastest first/last.
- **Two eggs from 100-floor building** — binary-search-like decreasing intervals.

---

## 11. Project / Resume Discussion

Use the **STAR** format: Situation → Task → Action → Result.
- Walk through your major project end-to-end.
- What was your specific contribution (vs teammates)?
- Architecture / tech stack and *why* those choices.
- Hardest bug + how you debugged it.
- What would you do differently now?
- Any deployment / production exposure?

**Anything on your resume is fair game.** If you wrote "Familiar with Spring Boot," expect Spring Boot questions.

---

## 12. HR Questions

- Tell me about yourself (90-second elevator pitch).
- Why Infosys? Why Specialist Programmer specifically?
- Why should we hire you?
- Strengths and weaknesses (be honest; weakness should have a fix in progress).
- Where do you see yourself in 5 years?
- Are you flexible with location, shift, technology?
- Tell me about a conflict and how you resolved it.
- Greatest achievement?
- Why did you choose this engineering branch?
- Any backlogs / academic gaps?
- Do you have offers from other companies?
- Do you have any questions for us? *(Always ask one — about training, tech stack, growth path.)*

---

## 13. Final Tips

1. **Practice on paper / whiteboard** — many SP interviews are now offline.
2. **Always dry-run** with a sample input — interviewers ask explicitly.
3. **State time + space complexity** after every solution.
4. **Brute force first, then optimize.** Don't sit silent.
5. **Talk while you code** — show your thought process.
6. **Master DP** — it's the differentiator between SE and SP.
7. **Master at least 2 SQL window functions** (RANK, DENSE_RANK).
8. **Striver's SDE Sheet + NeetCode 150** = ~90% topic coverage.
9. **Mock the HackWithInfy format** — 3 hrs, 3 problems, no breaks. Run it twice.
10. **Test edge cases** — empty input, single element, max/min values, negatives.

The SP role pays significantly more than standard Infosys SE — bar is higher, but worth it. Good luck!
