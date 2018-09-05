# Comparison object



{% tabs %}
{% tab title="Comparison object" %}
#### 1. Define operator&lt;\(\)

```text
bool operator<(T other) const
```

```text
struct Edge
{
    int from, to, weight;
    bool operator<(Edge other) const
    {
        return weight > other.weight;
    }
};
```

#### 2. Define a custom comparison function

```text
bool name(T a, T b)
```

```text
bool cmp(int a, int b)
{
    return occurrences[a] < occurrences[b];
}

sort(data.begin(), data.end(), cmp);
```

#### 3. Define operator\(\)\(\)

 A _functor_, or a function object, is an object that can behave like a function. This is done by defining **operator\(\)\(\)** of the class.

```text
struct cmp
{
    bool operator()(int a, int b)
    {
        return occurrences[a] < occurrences[b];
    }
};
```

```text
struct compare {
    bool operator()(const pair<int, string>& l, const pair<int, string>& r)
    {
        if (l.first == r.first) {
            return l.second < r.second;
        }
        else return l.first > r.first;
    }
};
```

#### 4. Use Lambda expression

First define the lambda object, then pass it to the template's type using `decltype` and also pass it directly to the constructor.

```text
auto comp = []( adjist a, adjlist b ) { return a.second > b.second; };
priority_queue< adjlist_edge , vector<adjlist_edge>, decltype( comp ) >
     adjlist_pq( comp );
```
{% endtab %}

{% tab title="Tip" %}
对于在类中使用sort算法进行排序，需要注意一些问题。

你很能会遇到这样的错误：   
sort函数出错“应输入 2 个参数，却提供了 3 个。

在类中你写了比较函数：

```text
bool compare_unique_ptr_int(  std::unique_ptr<int > &lhs,   std::unique_ptr<int > & rhs)
{
    return  *lhs < *rhs;
}
```

然后在类的某个成员函数中，使用了sort方法进行排序，第三个参数使用compare\_unique\_ptr\_int函数，这个时候就会出现上面所说的错误。

但是如何改进呢？   
方法有两种：

方法一：把compare\_unique\_ptr\_int函数改为静态方法：

```text
static bool compare_unique_ptr_int(  std::unique_ptr<int > &lhs,   std::unique_ptr<int > & rhs)
{
    return  *lhs < *rhs;
}

sort(v.begin(), v.end(), compare_unique_ptr_int);
```

方法二：使用lambda表达式进行

```text
sort(v.begin(), v.end(), [](std::unique_ptr<int > &lhs,   std::unique_ptr<int > & rhs)(){return  *lhs < *rhs;});
```
{% endtab %}
{% endtabs %}



