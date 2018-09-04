# 692. Top K Frequent Words

```text
class Solution { public:
    struct compare {
        bool operator()(const pair<int, string>& l, const pair<int, string>& r)
        {
            if (l.first == r.first) {
                return l.second < r.second;
            }
            else return l.first > r.first;
        }
    };
    
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> map;
        priority_queue<pair<int, string>, vector<pair<int, string>>, compare > pq;
        vector<string> res;
    
        for ( int i = 0; i < words.size(); i++ ) {
            map[words[i]]++;
        }
    
        for ( auto it = map.begin(); it != map.end(); ++it ) {
            pq.push(make_pair(it->second, it->first));
            if (pq.size() > k) {
                pq.pop();
            }
        }
    
        while(pq.size() != 0) {
            res.push_back(get<1>(pq.top()));
            pq.pop();
        }
        reverse(res.begin(), res.end());
    
        return res;
    }
};
```

