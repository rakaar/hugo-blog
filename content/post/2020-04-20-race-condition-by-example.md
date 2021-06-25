---
date: "2020-04-20T00:00:00Z"
subtitle: An example from Udacity OS course
tags:
- OS
- threads
- Concurrency
title: Race Condition by example
---


### Consider the following code

```
#include <stdio.h>
#include <pthread.h>

#define NUM_THREADS 4

void *threadFunc(void *pArg) { /* thread main */
	int *p = (int*)pArg;
	int myNum = *p;
	printf("Thread number %d\n", myNum);
	return 0;
}

int main(void) {
	int i;
	pthread_t tid[NUM_THREADS];
	
	for(i = 0; i < NUM_THREADS; i++) { /* create/fork threads */
		pthread_create(&tid[i], NULL, threadFunc, &i);
	}
	
	for(i = 0; i < NUM_THREADS; i++) { /* wait/join threads */
		pthread_join(tid[i], NULL);
	}
	return 0;
}
```

In the above example, one would normally expect that the output of the program would be
```
Thread Number 0
Thread Number 1
Thread Number 2
Thread Number 3
```
Or the above in a jumbled order because we cannot guarantee the order of execution of print.
Now there is also this condition possible where we can expect a  result something like this
```
Thread Number 0
Thread Number 2
Thread Number 2
Thread Number 3
```
Now how is that possible ?

The main reason is that the variable i is available for change even while its value is being read.
In detail,  suppose we reach a condition where i = 1 ; and `threadFunc` starts getting exectuted.
As per the code of `threadFunc`, the `pointer p` is assigned the value of address of `i`, and variable `myNum` is being assigned the value of variable whose address is stored by `p pointer`, which we expect to be value of `i`, and hence you expect Thread Number 1 to be printed.
But here before printing that, the value of  `i` has been changed by the `main` thread to 2 by incrementing  it. So when you are expecting that you are assigning 1 to `myNum` because `p` contains address of `i`, whose value is 1, is actually no longer 1 but has been incremented to 2; by the `main thread`.


This is called Data Race or Race condition where a variable is being read by one thread, and the variable's value is being written(altered) by other thread.
Now how do we avoid it
Store it in a variable(an array preferably here) which isn't altered and pass it as argument in pthread_create.  Because though i will be altered the value which we want has been copied into a array cell which is not going to be changed. The code would be in this manner:


```
#include <stdio.h>
#include <pthread.h>

#define NUM_THREADS 4

void *threadFunc(void *pArg) { /* thread main */
	int myNum = *((int*)pArg);
	printf("Thread number %d\n", myNum);
	return 0;
}

int main(void) {

	int i;
	int tNum[NUM_THREADS];
	pthread_t tid[NUM_THREADS];
	
	for(i = 0; i < NUM_THREADS; i++) { /* create/fork threads */
		tNum[i] = i;
		pthread_create(&tid[i], NULL, threadFunc, &tNum[i]);
	}
	
	for(i = 0; i < NUM_THREADS; i++) { /* wait/join threads */
		pthread_join(tid[i], NULL);
	}

	return 0;
}
```

