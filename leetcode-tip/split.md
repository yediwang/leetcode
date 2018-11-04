# Split

```text
    vector<string> split(string str, string pattern) {
        size_t pos;
        vector<string> result;
        int size = str.size();

        for(int i = 0; i < size; i++)
        {
            pos = str.find(pattern, i);
            if((int)pos < size)
            {
                string s = str.substr(i, pos-i);
                result.push_back(s);
                i = pos + pattern.size() - 1;
            }
        }
        return result;
    }
```



