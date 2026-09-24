# streak-continue-
day 12 
class Solution {
    struct Node {
        int prod;
        int cnt[5];

        Node(int k = 5) {
            prod = 1;
            for (int i = 0; i < 5; i++)
                cnt[i] = 0;
        }
    };

    int k;
    vector<Node> tree;

    Node merge(Node &L, Node &R) {
        Node res;

        // Product of complete segment
        res.prod = (L.prod * R.prod) % k;

        // Prefixes completely inside left segment
        for (int r = 0; r < k; r++) {
            res.cnt[r] = L.cnt[r];
        }

        // Prefixes that take all of left
        // and some prefix of right
        for (int r = 0; r < k; r++) {
            int newRem = (L.prod * r) % k;
            res.cnt[newRem] += R.cnt[r];
        }

        return res;
    }

    void build(int node, int l, int r, vector<int>& nums) {
        if (l == r) {
            int rem = nums[l] % k;

            tree[node].prod = rem;
            tree[node].cnt[rem] = 1;

            return;
        }

        int mid = (l + r) / 2;

        build(node * 2, l, mid, nums);
        build(node * 2 + 1, mid + 1, r, nums);

        tree[node] = merge(tree[node * 2], tree[node * 2 + 1]);
    }

    void update(int node, int l, int r, int idx, int val) {
        if (l == r) {
            int rem = val % k;

            tree[node] = Node();
            tree[node].prod = rem;
            tree[node].cnt[rem] = 1;

            return;
        }

        int mid = (l + r) / 2;

        if (idx <= mid)
            update(node * 2, l, mid, idx, val);
        else
            update(node * 2 + 1, mid + 1, r, idx, val);

        tree[node] = merge(tree[node * 2], tree[node * 2 + 1]);
    }

    Node query(int node, int l, int r, int ql, int qr) {
        // Completely inside
        if (ql <= l && r <= qr)
            return tree[node];

        int mid = (l + r) / 2;

        // Only left
        if (qr <= mid)
            return query(node * 2, l, mid, ql, qr);

        // Only right
        if (ql > mid)
            return query(node * 2 + 1, mid + 1, r, ql, qr);

        // Both sides
        Node L = query(node * 2, l, mid, ql, qr);
        Node R = query(node * 2 + 1, mid + 1, r, ql, qr);

        return merge(L, R);
    }

public:
    vector<int> resultArray(vector<int>& nums, int K,
                            vector<vector<int>>& queries) {

        k = K;

        int n = nums.size();

        tree.resize(4 * n);

        build(1, 0, n - 1, nums);

        vector<int> ans;

        for (auto &q : queries) {

            int index = q[0];
            int value = q[1];
            int start = q[2];
            int x = q[3];

            // Permanent update
            update(1, 0, n - 1, index, value);

            // Query [start ... n-1]
            Node res = query(1, 0, n - 1, start, n - 1);

            ans.push_back(res.cnt[x]);
        }

        return ans;
    }
};

day 14 
class Solution {
public:
    int minOperations(vector<int>& nums, int x) {
        int total = 0;

        for (int num : nums) {
            total += num;
        }

        int target = total - x;

        // We need to find the longest subarray
        // whose sum is equal to total - x.
        if (target < 0)
            return -1;

        int left = 0;
        int sum = 0;
        int maxLen = -1;

        for (int right = 0; right < nums.size(); right++) {
            sum += nums[right];

            while (sum > target && left <= right) {
                sum -= nums[left];
                left++;
            }

            if (sum == target) {
                maxLen = max(maxLen, right - left + 1);
            }
        }
day 14
class Solution {
public:
    int digitSum(int n) {
        int sum = 0;

        while (n > 0) {
            sum += n % 10;
            n /= 10;
        }

        return sum;
    }

    int smallestIndex(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            if (digitSum(nums[i]) == i) {
                return i;
            }
        }

        return -1;
    }
};
        if (maxLen == -1)
            return -1;

        return nums.size() - maxLen;
    }
};
