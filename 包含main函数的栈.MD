##（一）题目描述
  定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

##（二）思路分析
  当最小元素被弹出栈的时候，为了得到下一个最小的元素，所以应该保留次小元素。
  即定义两个栈，一个数据栈，一个辅助栈。
  数据栈存放每次添加的数值；辅助栈存放之前的最小元素与新压入栈元素两者的较小值

##（三）代码实现



    class Solution {
    public:
        stack<int> sta_data;
        stack<int> sta_min;
    
        void push(int value) {
        sta_data.push(value);
            if((sta_min.size() == 0) || (value < sta_min.top()))
            {
                sta_min.push(value);
            }
            else
            {
                sta_min.push(sta_min.top());
            }
        }
        void pop() {
            assert((sta_data.size() > 0) && (sta_min.size() > 0));
            sta_data.pop();
            sta_min.pop();
        }
        int top() {
            assert(sta_min.size() > 0);
            return sta_min.top();
        }
     
        int min() {
            assert((sta_data.size() > 0) && (sta_min.size() > 0));
            return sta_min.top();
        }
    };