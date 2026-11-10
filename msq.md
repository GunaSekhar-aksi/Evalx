## Question 1

```JAVA
import java.util.*;

class Main {
    public static void solve(int[] arr) {
        int n = arr.length;
        int[] prev = new int[n];
        int[] next = new int[n];
        boolean[] isDeleted = new boolean[n];

        for (int i = 0; i < n; i++) {
            prev[i] = i - 1;   // -1 = nobody on left
            next[i] = i + 1;   //  n = nobody on right
        }

        Integer[] idx = new Integer[n];
        for (int i = 0; i < n; i++) idx[i] = i;
        Arrays.sort(idx, (a, b) -> Integer.compare(arr[a], arr[b]));

        long ans = 0;

        for (int pos : idx) {
            if (isDeleted[pos]) continue;

            ans += arr[pos];

            int left = prev[pos];
            int right = next[pos];

            isDeleted[pos] = true;
            if (left >= 0) isDeleted[left] = true;
            if (right < n) isDeleted[right] = true;

            int newLeft = (left >= 0) ? prev[left] : -1;
            int newRight = (right < n) ? next[right] : n;

            if (newLeft >= 0) next[newLeft] = newRight;
            if (newRight < n) prev[newRight] = newLeft;
        }

        System.out.println(ans);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();
        solve(arr);
        sc.close();
    }
}
```


## Question 2

```Java
// Online Java Compiler (Editor)
// Write and run Java online using this editor.
import java.util.*;

class Main {

    public static void solve(int[] arr1, int[] arr2) {
        Stack<Integer> st = new Stack<>();
        for(int i = 0 ; i < arr1.length ; i++) {
            int ball = arr1[i] * arr2[i];

            if(ball > 0) st.push(i);
            else {
                while(!st.isEmpty() && (arr1[st.peek()] * arr2[st.peek()]) > 0 && (arr1[st.peek()] * arr2[st.peek()]) < Math.abs(ball)) {
                    st.pop();
                }
                if(!st.isEmpty() && (arr1[st.peek()] * arr2[st.peek()]) == Math.abs(ball)) {
                    st.pop();
                } else if(st.isEmpty() || (arr1[st.peek() * arr2[st.peek()]]) < 0) {
                    st.push(i);
                }
            }
        }

        System.out.println(st);
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[] arr1 = new int[n];
        int[] arr2 = new int[n];
        
        for (int i = 0; i < n; i++) {
            arr1[i] = sc.nextInt();
        }
        for(int i = 0 ; i < n ; i++) {
            arr2[i] = sc.nextInt();
        }
        

        solve(arr1, arr2);

        sc.close();
    }
}
```
 
