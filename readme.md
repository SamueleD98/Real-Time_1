# Assignment 1 REAL-TIME OPERATING SYSTEMS

Starting from the exercise with 3 tasks scheduled with Rate Monotonic, an application with 4 periodic tasks has been created.  

First thing has been done was to add the fourth task and its characteristic function. Then the three global variables have been declared:  
<pre>
int T1T2 = 0;  
int T1T4 = 0;  
int T2T3 = 0;  
</pre>
It's necessary to declare the three mutexes attributes and set their protocol as Priority Ceiling:  
<pre>
int pthread_mutexattr_init(pthread_mutexattr_t &attr1);  
int pthread_mutexattr_setprotocol(pthread_mutexattr_t &attr1, int PTHREAD_PRIO_PROTECT);  
int pthread_mutexattr_init(pthread_mutexattr_t &attr2);  
int pthread_mutexattr_setprotocol(pthread_mutexattr_t &attr2, int PTHREAD_PRIO_PROTECT);  
</pre>
The first two mutexes have the same priority ceiling as they both are associated to the thread with the highest priority. So they both relate to attr1, meanwhile the third mutex relate to attr2.  

After updating the tasks periods, the U computation algorithm needs to be updated in order to take account of the fourth task.  
In the original code _parameters[i].sched_priority_ keeps the priority of eache thread. Its values are needed to set the priority ceiling of the two attributes and so of the three mutexes:  
<pre>
int prioceiling1 = parameters[0].sched_priority;  
int prioceiling2 = parameters[1].sched_priority;  
	  
int pthread_mutexattr_setprioceiling(pthread_mutexattr_t &attr1, int prioceiling1);  
int pthread_mutexattr_setprioceiling(pthread_mutexattr_t &attr2, int prioceiling2);  
  
int pthread_mutex_init(pthread_mutex_t &mutex12, const pthread_mutexattr_t &attr1);  
int pthread_mutex_init(pthread_mutex_t &mutex14, const pthread_mutexattr_t &attr1);  
int pthread_mutex_init(pthread_mutex_t &mutex23, const pthread_mutexattr_t &attr2);  
</pre>  
After the U computation algorithm has been run, the global variables value has been modified as the tasks code does exactly that. Although it is not necessary for the purpose of the application (which is simply to write and read from the variables), their values are again set to zero.  

The threads are then created and executed. Just before the end of the main, the mutexes and their attributes are destroyed:  
<pre>
int pthread_mutex_destroy(pthread_mutex_t *mutex12);  
int pthread_mutex_destroy(pthread_mutex_t *mutex14);  
int pthread_mutex_destroy(pthread_mutex_t *mutex23);  
int pthread_mutexattr_destroy(pthread_mutexattr_t *attr1);  
int pthread_mutexattr_destroy(pthread_mutexattr_t *attr2);  
</pre>
Each task reads or writes global variables only after it calls the mutex. Below, the second task takes control of two mutexes, one for reading and one for writing:  
<pre>
int pthread_mutex_lock(pthread_mutex_t &mutex12 );  
  double temp=T1T2;  
  printf(" ");  
  printf("%f", temp);  
  printf(" ");  
int pthread_mutex_lock(pthread_mutex_t &mutex12 );  
  
int pthread_mutex_lock(pthread_mutex_t &mutex23 );  
  T2T3++;  
int pthread_mutex_lock(pthread_mutex_t &mutex23 );  
</pre>






























