# Editorial for CSE 4304: Data Structures Lab

<details>
<summary>Problem A - Bangladesh Parliament</summary>

<details>
<summary>Hint 1</summary>

Can you understand the seating arrangement from the input?

</details>

<details>
<summary>Hint 2</summary>

What will happen if you reverse the input?

</details>

<details>
<summary>Solution</summary>

The seating arrangement of the MPs form a binary search tree. In the odd sessions, the speech order follows a post-order traversal of the tree. In the even session, the speech order follows a modified post-order traversal where the right subtree is visited before the left subtree.

Had you been given the tree, you could have found the even session speech order easily. But since you are given the odd session speech order, you have to build the tree from it first.

To build the same binary search tree, you need to insert the root first and make sure that the parent of any other node is inserted before it. Here you need to make an observation.

The observation is, in the odd session speech order the children will always come before the parent (because it follows post-order traversal). So, if you reverse the speech order, you'll get the parent before the children. With this order, you can now build the same binary tree that describes the seating arrangement of the MPs.

So to solve the problem, you need to reverse the given order, insert the nodes one by one to create a BST and then perform the traversal in the described manner.

</details>
</details>


<details>
<summary>Problem B - Berries</summary>

<details>
<summary>Solution</summary>

If a berry is in the $i$-th group, there are exactly $(i-1)$ groups before it.

You can store the label of the last berry of the each group in an array. For every group, you can get the label of the last berry by adding the number of berries in that group and all groups before it (Prefix Sum). After that, whenever you're asked in which group the berry $x$ is in, you can simply answer it by telling the number of elements in this prefix sum array that are strictly smaller than $x$.

Since the array size is at most $1e5$ and you have to answer $1e5$ queries, for every query you can not traverse the array and count the number of elements one by one (an algorithm with a time complexity of $O(nm)$ is too slow for this problem). What you can do is perform binary search!

With binary search, you can answer each query in $O(log(n))$ time. For m queries, the total time complexity will be $O(mlog(n))$. You can write the code for binary search on your own or use the STL function lower_bound(). Or you can save the values of the prefix sum array in an ordered set and use the order_of_key() function to answer the queries.

</details>

<details>

<summary>Alternate Solution</summary>

Since the total number of berries is only $1e6$ at most, you can simply save the group number of every berry in an array. From this array, you can tell which berry is in which group in $O(1)$ time.

</details>
</details>


<details>
<summary>Problem C - One Button</summary>

<details>
<summary>Hint</summary>

Which data structure follows a Last In First Out (LIFO) order?

</details>

<details>
<summary>Solution</summary>

This is a straightforward Stack problem. When an alphabet key is pressed, you push it onto the stack, and when '<' is pressed, you pop a character from the stack.

The Stack operates on the Last In First Out (LIFO) principle, so the top of the stack always contains the last character in the string. To print the string, you need to output the characters from the stack in the reverse order of their popping sequence.

Pro Tip: By using the push_back() and pop_back() function of the string data structure in C++, you can emulate the task of a stack and finally print the string without needing to reverse it.

</details>
</details>


<details>
<summary>Problem D - Three Buttons</summary>

<details>
<summary>Hint</summary>

Use Linked List.

</details>

<details>
<summary>Solution</summary>

This is probably the most annoying problem of the contest.

You can use a pointer or iterator to point to the cursor and insert or remove elements. However, as it is an implementation problem, it is more difficult to code than understand. So a sample implementation is given:

</details>

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long
#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
#define endl "\n"



void pre()
{
    fastio;

    
}

void solve(int tc)
{
    string str;
    getline(cin, str);

    list<char> linked_list;
    auto it=linked_list.begin(); // Declaring iterator.

    int i, n=str.size();
    for(i=0; i<n; i++)
    {
        // Delete character
        if(str[i]=='<')
        {
            if(it==linked_list.begin()) continue; // Can't delete from here.
            it=linked_list.erase(--it); // Delete the previous character.
        }

        // Jump
        else if(str[i]=='[') it=linked_list.begin();
        else if(str[i]==']') it=linked_list.end();

        // Insert character
        else
        {
            it=linked_list.insert(it, str[i]);
            it++;
        }
    }

    // Print all characters of the list.
    for(auto ch: linked_list) cout << ch;
}

signed main()
{
    pre();

    int tc, tt=1;
    cin >> tt;

    cin.ignore(); // Ignore the newline after the number of testcases.
    
    for(tc=1; tc<=tt; tc++)
    {
        solve(tc);
        cout << endl;
    }

    return 0;
}
```

</details>

<details>
<summary>Alternate Solution</summary>

If the '[' and ']' characters (let's call these jumps) had not been here, this would have been almost the same problem as $C$ (not exactly same because you may have to ignore some deletions here in case they are invalid).

You can try solving the problem $C$ and when you encounter a jump, you store the result and go to solve a new 'segment'.

Depending on where you jump from, you have to add your segment to the front or back of your answer. You can manage it with a double ended queue or 'deque'.

</details>

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long
#define fastio ios_base::sync_with_stdio(0); cin.tie(0)
#define endl "\n"



void pre()
{
    fastio;


}

void solve(int tc)
{
    string str, segment;
    getline(cin, str);

    deque<string> dq;
    int i, n=str.size(), flag=0; // flag=0 means pointer is at back, flag=1 means otherwise.
    for(i=0; i<n; i++)
    {
        // Delete character
        if(str[i]=='<')
        {
            // If the current segment is not empty, delete its last character.
            if(!segment.empty()) segment.pop_back();

            // If the current segment is empty, delete from the previous segment.
            else if(!dq.empty() && flag==0)
            {
                dq.back().pop_back();
                if(dq.back().empty()) dq.pop_back(); // If segment becomes empty, remove it from deque.
            }
        }

        // Jump
        else if(str[i]=='[' || str[i]==']')
        {
            if(!segment.empty()) // No need to add empty segments.
            {
                if(flag==0) dq.push_back(segment);
                else dq.push_front(segment);
            }

            segment=""; // Initializing new segment.

            if(str[i]==']') flag=0;
            else flag=1;
        }

        // Insert character
        else segment.push_back(str[i]);
    }

    // Add the last segment.
    if(flag==0) dq.push_back(segment);
    else dq.push_front(segment);

    n=dq.size();
    for(i=0; i<n; i++) cout << dq[i];
}

signed main()
{
    pre();

    int tc, tt=1;
    cin >> tt;

    cin.ignore(); // Ignore the newline after the number of testcases.

    for(tc=1; tc<=tt; tc++)
    {
        solve(tc);
        cout << endl;
    }

    return 0;
}
```

</details>
</details>


<details>
<summary>Problem E - Fine Music Taste</summary>

<details>
<summary>Hint 1</summary>

Suppose your playlist contains some compositions. What will happen if you add a new one?

Will it always increase the pleasure, always decrease the pleasure or does it depend on something?

</details>

<details>
<summary>Hint 2</summary>

Adding a new composition to a playlist always increases its length.

</details>

<details>
<summary>Hint 3</summary>

When will the smallest beauty value of a playlist change?

</details>

<details>
<summary>Hint 3.5</summary>

If adding a new composition to a playlist doesn't change the smallest beauty value of the playlist, it will always increase the pleasure.

</details>

<details>
<summary>Hint 4</summary>

Is there any reason to leave out the most beautiful composition from your playlist?

</details>

<details>
<summary>Hint 4.5</summary>

Is there any reason to replace one of the compositions of your playlist with a less beautiful one?

</details>

<details>
<summary>Solution</summary>

At first let's simplify the problem by ignoring the constraint of the maximum playlist size. Here, playlist size means the number of compositions in the playlist, which is given as $k$ in the problem statement. In the simplified version of the problem, you can keep as many compositions as you want.

You need to sort the compositions by their beauty in decreasing order. Now you have the most beautiful composition in position $0$ and the least beautiful composition in position $(n-1)$.

Now, if you keep any composition in your playlist, you must also keep all the compositions before it (because they will increase the time length leaving the smallest beauty unchanged). So, the optimal playlist will be a prefix of this sorted array.

There are exactly $n$ options for the size of the prefix. You can check all the options.

You need to traverse the array once, keep a running sum of length and a running min of beauty (since the array is sorted, the latest composition you're picking is the least beautiful), calculate the pleasure for every prefix and pick the best answer.

Now let's introduce the playlist size constraint again. You can't keep more than $k$ compositions in your playlist.

Just like before, you need to traverse your sorted array and add the compositions to your playlist one by one. In every iteration, calculate the pleasure and compare it with the maximum pleasure discovered yet.

There is a slight difference this time. When your composition size becomes greater than $k$, you have to remove the extra composition. Since the minimum beauty of your playlist is only determined by the latest composition, it is always optimal to delete the shortest composition. To keep track of the shortest composition in the playlist at any time, use a heap (priority_queue). Traverse the full array like this and pick the best answer.

</details>
</details>
