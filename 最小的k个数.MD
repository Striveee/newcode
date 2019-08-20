#题目描述
<font face="微软雅黑" size=4 color=brown>
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。
</font>
#方法一（最简单的思路）
<font face="微软雅黑" size=4 color=brown>
先进行从小到大排序，然后再将数组的前四个取出来</br>
时间复杂度：O(nlogn)
</font>

    class Solution {
    public:
        vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
            vector<int> res;
            if(input.size() < k || k<=0)
            {
                return res;
            }
            sort(input.begin(), input.end());
            for(int i=0; i<k; i++)
            {
                res.push_back(input[i]);
            }
            return res;
        }
    };
#方法二
<font face="微软雅黑" size=4 color=brown>
快排思路：每选取一个数（待排列数据的第一个数据）作为比较的基准，然后从后往前找比基准小的，将其放在前面空缺位置，其次从前往后找比基准大的数据放在后面空缺位置，直到 i 和 j 相遇。将基准放入 i 位置，基准前的数据都比基准小，基准后的数据都比基准大，然后递归分别处理左半边和右半边。</br>
题目思路：基于快速排序Partition函数来解决这个问题。如果基于数组的第k个数字来调整，使得比第k个数字小的所有数字都位于数组的左边，比第k个数字大的所有数字都位于数组的右边。这样调整之后，位于数组中左边的k个数字就是最小的k个数字（这k个数字不一定是排序的）。</br>
时间复杂度O(n)
</font>

    class Solution {
    public:
        vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
            vector<int> output;
            int len = input.size();
            if(k>len || len==0 || k<=0)
            {
                return vector<int>();
            }
            int start = 0;  //左指针
            int end = len-1;  //右指针
            int temp = partition(input, start, end);   //基准
            while(temp != k-1)  //排序k次,退出条件：找到前k小的数
            {
                if(temp > k-1)   //基准的左半部分,即第k个数在index的左边
                {
                    end = temp - 1;
                    temp = partition(input, start, end);
                }
                else   //基准的右半部分
                {
                    start = temp + 1;
                    temp = partition(input, start, end);
                }
            }
            //将前k个数取出来
            vector<int> res(input.begin(), input.begin() + k);
            return res;
        }
    private:
        //快速排序的partition函数
        int partition(vector<int> &input, int left, int right)
        {
            int pivotkey = input[left];
            while (left < right)
            {
                while (left < right && pivotkey <= input[right]) --right;
                input[left] = input[right];
                while (left < right && pivotkey >= input[left]) ++left;
                input[right] = input[left];
            }
            input[left] = pivotkey;
            return left;
        }
    };

##方法三(适合处理海量数据)
<font face="微软雅黑" size=4 color=brown>
不必一次载入所有的数据到内存，适合从海量的数据中找出最小的k个数
思想：用multiset保存最小的k个数字。每次读入一个数，如果容器中已有的数字少于k个，则直接把这次读入的整数放入容器之中；如果容器中已有k个数字，则比较读入的数字和容器中最大的数字，如果前者大，则在容器中用前者替换后者；如果后者大，则前者不可能是最小的k个数之一。</br>
时间复杂度O(nlogk)
</font>

    //采用从大到小排序。默认的是less<key>，默认采用的是递增排序
    typedef multiset<int, greater<int>> intSet; 
    typedef multiset<int, greater<int>>::iterator setIterator;
    
    //在data中寻找最小的k个数保存到leastNumbers中
    void getLeastNumbers(const vector<int> &data, intSet &leastNumbers, int k)
    {
        leastNumbers.clear();
        if (k<1 || data.size() < k) 
        return;
        vector<int>::const_iterator it = data.begin();
        for (;it!=data.end();++it)
        {
            //如果leastNumbers中少于k个数
            if (leastNumbers.size() < k)
            {
                leastNumbers.insert(*it);
            }
            else
            {
                //如果该数比leastNumbers中最大的数小，则替换
                if (*it < *(leastNumbers.begin()))
                {
                    leastNumbers.erase(leastNumbers.begin());
                    leastNumbers.insert(*it);
                }
            }
        }
    }