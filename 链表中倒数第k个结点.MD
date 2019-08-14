##链表中倒数第K个结点
输入一个链表，输出该链表中倒数第k个结点。

    /*
    struct ListNode {
    	int val;
    	struct ListNode *next;
    	ListNode(int x) :
    			val(x), next(NULL) {
    	}
    };*/
    //思想：定义两个快慢指针，pa先走k-1步，pb指向头结点开始同时向后移动，
    //当pa移动到最后一个节点的时候，此时pb所指向的结点即为所求的daoshudik个结点
    class Solution {
    public:
        ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        //排除意外情况
            if(pListHead == NULL || k == 0)
            {
                return NULL;
            }
    
            ListNode *pa = pListHead;
            ListNode *pb = NULL;
    
            for(unsigned int i=0; i<k-1; i++)
            {
                if(pa->next != NULL)
                {
                    pa = pa->next;
                }
                else
                {
                    return NULL;
                }
            }
            pb = pListHead;
            while(pa->next != NULL)
            {
                pa = pa->next;
                pb = pb->next;
            }
            return pb;
        }
    };