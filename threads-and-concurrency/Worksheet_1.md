# 🧵 Concurrency Worksheet #1🧵
# Background
In the early days of computing, computers typically processed a sequence of instructions only one at a time, and in order. They weren't capable of running lots of programs at once, unlike the computers of today. Concurrency dates back all the way to 1965, where a system named the "Berkeley Timesharing System" was being developed in the University of California, Berkeley. This was a revolutionary system; it allowed for sequences of instructions to be interleaved between one another, so that it seemed that multiple programs were being run at once!

So, what is a thread? 
A thread can be thought of as a single set of instructions, that can run in parallel with many other threads. It's similar to how multiple threads of cotton can be interwoven together to make a piece of fabric. Sounds simple right? There's a whole world of research out there, and in these worksheets, we will scratch the surface of thius wide and wonderful world.

Modern operating systems such as Windows, Mac and Linux all handle lots of different threads when running programs. For example, even your web browser will use multiple threads - perhaps one for displaying the UI, and one for handling network requests (such as downloading the webpage, streaming a video, etc.). 

If you've researched buying different laptops or CPUs, you may have noticed it might say something like "4 cores, 8 threads". This means that the CPU is optimized to run 8 threads at a time, making your programs feel even smoother! Of course, your computer can run almost an infinite number of threads in theory; it will simply divide up the time spent on each core to the multiple threads it has to process. A very basic example might be to allocate 1 second for one thread, and then switch another thread for another second, and so on. In practice, threads are switching so quickly that you will probably not notice this!


# Let's make a thread! 🧵

Open up IntelliJ and start a new project. If you're unsure on how to do this, feel free to refer to the supplementary sheet to help you set up your environment!

## 1) Make a counter 🔢

1) Create a new class named `Counter`
2) Create a `public void` method named `run()`. Using a for loop, count up from 1 to 3, printing out the value of the counter in each iteration

After the above the steps, you should have something similar to below:
```
📦Project
 ┣ 📂.idea
 ┣ 📂out
 ┣ 📂src
 ┗ ┗ 📜Counter
```
Counter:
```java
public class Counter {  
  
 public void run() {  
    for (int i = 1; i <= 3; i++) {
      System.out.println("i is: " + i);
    }
  }  

}
```

## 2) Create your Main class
1) Create a new class named `Main`
2) Inside the `public static void main` function, create a new `Counter` object
3) Call the `run` method twice on the newly made counter

You should now have something similar to below:
```
📦Project
 ┣ 📂.idea
 ┣ 📂out
 ┣ 📂src
 ┗ ┣ 📜Main
   ┗ 📜Counter
```
```java
public class Main {  
  public static void main(String[] args) {  
    Counter counter = new Counter();  
    counter.run();  
    counter.run();  
  }  
}
```

Try running it! Try running it again! And again, and again...
Do you think it is going to change? Why do you think that is?

## 3) Converting the counter so it can be run as a thread

1) In the Counter class, make the Counter class `implement Runnable`
2) Add an `@override` tag to the run method

You should now have something similar to below:

Counter:
```java
public class Counter implements Runnable {
  @Override
  public void run() {
    for (int i = 1; i <= 3; i ++) {
      System.out.println("i is: " + i);
    }
  }
}
```
> 💡 What is this doing? 
> 
> When a thread implements Runnable, it is saying the class's run() method can be run by a thread. We will see how to make threads from this in the next section!

## 4) Convert Main to create Counter threads

Modify your Main.java so it matches the below: 
```java
public class Main {
  public static void main(String[] args) {
    Counter counter = new Counter();

    /* Create two new threads from the Counter class */
    Thread t1 = new Thread(counter);
    Thread t2 = new Thread(counter);
    
    /* Tell the threads to start running */
    t1.start();
    t2.start();
    
    try {
      /* Wait for the threads to finish */
      t1.join();
      t2.join();
    } catch (InterruptedException e) {
      System.out.println("Execution interrupted!");
    }
  }
}
```
> 💡 How does this code work? 
> 
> When we create threads t1 and t2, they are like mini programs that can count up to 3 (which is what we wrote in the run() method). By starting the threads, we are telling the program to start running the threads. 
> 
> What does .join() do? 
> 
> There is actually a third thread in this program - the thread running the Main class! By joining on both threads, we are saying to stop and wait for both of them to finish, otherwise the Main thread may exit early before the other threads have time to finish. We have to wrap this with a try/catch block in case something unexpected happens and the threads t1 and t2 get interrupted.


Try running the new Main now! And again, and again, and again... What do you notice? Does it change?

# You try!
- Modify the program so that the thread does something different from counting up to 10 (perhaps it prints “Hello world” multiple times).

- Can you extend the program to make 3 different counters count up to 10 simultaneously? Try running it multiple times :)

- Can you modify the program to make each thread count up to a random number between 10 and 20?
- Can you extend the program to specify (through a command line argument) an arbitrary number of counters? (You may want to search up how to get command line arguments in Java)

# Further exploration 🔍

- What might cause the differences in output when we run the program each time?
- Can you find out how to create threads in another programming language of your choice (Python, C#, C++, etc.)? What are the differences?