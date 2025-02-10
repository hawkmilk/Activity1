// Activity1

/* 2. I chose this Method because I'm more Aquanited with for Loops, and It's what happened to Be the Easiest to Impliment from What I've Researched.

3.The Hardest Part was Getting Started With the For Loop, Because Since It's been a while since I've coded, I had to warm up once again, But after figuring Out how to get The Numbers In order going up, It didn't take as long to Reverse the process.

*/

package packageArrays;

import java.util.*;

public class ArraySorterTest { public static void sortArray(int[] myArr, int arrSize) { Arrays.sort(myArr, 0, arrSize);

    for (int i = 0; i < arrSize / 2; i++) {
        int a = myArr[i];
        myArr[i] = myArr[arrSize - 1 - i];
        myArr[arrSize - 1 - i] = a;
    }

}


public static void main(String[]args){
    Scanner scanner = new Scanner(System.in);
    int arrSize = scanner.nextInt();
    int[] myArr = new int[20];

    for (int i = 0; i < arrSize; i++) {
        myArr[i] = scanner.nextInt();
    }

    sortArray(myArr, arrSize);
    for (int i = 0; i < arrSize; i++) {
        System.out.print(myArr[i] + ",");
    }

}
}
