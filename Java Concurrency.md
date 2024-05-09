# Java Multithreading (Concurrency)

## Questions, to think about?
- [x] Why do we need to use Thread and even Multi-threading concepts, and how to create a thread in Java?
- [x] How threads are executed?
- [x] What happens, if we directly call run() method instead of start() method in Java?
- [x] What is the join() method and when to use it?
- [x] What is the main thread? 
- [x] What are synchronized blocks and methods and why do we need to use these in our code?
- [x] What is Thread Life Cycle? What are the different states of the Thread Life Cycle?
- [x] What is Inter Thread Communication (ITC) in Java?
- [x] What is the Deposit and Withdraw problem (P1) that comes under Inter Thread Communication (ITC)?
- [x] What is the Producer and Consumer problem (P2) that comes under Inter Thread Communication (ITC)?

## 1. Why do we need to use Thread and even Multi-threading concepts, and how to create a thread in Java?

_**Multi-threading:**_
Multi-threading is a concept of executing two or more threads simultaneously/concurrently to maximize CPU utilization.

_**Thread:**_
A Thread is a very lightweight process, or we can say it is the smallest part of the process that allows a program to operate more efficiently by running multiple tasks simultaneously. 

![image](https://github.com/AkashChauhanSoftEngi/Java_Programming_Practice/assets/23252844/8a965e9d-6aa1-44b1-abd3-0a3c569edfbf)

**Code-1:** Try to identify the problem with this code.
```java
public class SampleClass {

	public static void main(String[] args) {
		
		C1 obj1 = new C1();
		C2 obj2 = new C2();
		
		obj1.show1();
		obj2.show2();
		
		System.out.println("Done with the entire work!");
	}
}

class C1 {
	void show1() {

		Scanner obj = new Scanner(System.in);
		System.out.println("Enter the value of a: ");
		int a = obj.nextInt();
		
		System.out.println(a);
		obj.close();
	
	}
}

class C2 {
	void show2() {
		for (int i = 1; i <= 1000; i++) {
			System.out.println(i);
		}
	}
}
```

**Code-2:** Solution of the above problem. (Multi-Threading Applied)
```java
public class SampleClass {

	public static void main(String[] args) {
		
		Thread t1 = new Thread(new TC1(), "tc1");
		Thread t2 = new Thread(new TC2(), "tc2");
		
		t1.start();
		t2.start();
		
		System.out.println("Done with the entire work!");
	}
}

class TC1 extends Thread{
	public void run() {

		Scanner obj = new Scanner(System.in);
		System.out.println("Enter the value of a: ");
		int a = obj.nextInt();
		
		System.out.println(a);
		obj.close();
	
	}
}

class TC2 extends Thread{
	public void run() {
		for (int i = 1; i <= 1000; i++) {
			System.out.println(i);
		}
	}
}
```

## 2. How threads are executed?
> 1. Multi-threading follows the concurrent execution of multiple threads.
> 2. Suppose we have three threads: T1, T2 and T3. All the above threads required at least 1 minute to execute the associative task completely.
> 3. Then these threads will be executed concurrently like: T1,T3,T1,T2,T3,T2 {on each turn, 30 secs provided to each thread}

## 3. What happens, if we directly call run() method instead of start() method in Java?
> 1. If you call the run() method directly instead of using start() , the code in the run() method will be executed in the current thread of execution rather than in a new thread.
> 2. This means that the code will be executed sequentially in the current thread, without creating a new thread of execution.

```java
public class SampleClass {

	public static void main(String[] args) {
		
		Thread t1 = new Thread(new TC1(), "tc1");
		Thread t2 = new Thread(new TC2(), "tc2");
		
		t1.run();
		t2.run();
		
		System.out.println(Thread.currentThread().getName());
		System.out.println("Done with the entire work!");
	}
}

class TC1 extends Thread{
	public void run() {

//		Scanner obj = new Scanner(System.in);
//		System.out.println("Enter the value of a: ");
//		int a = obj.nextInt();
//		
//		System.out.println(a);
//		obj.close();

		System.out.println(Thread.currentThread().getName());

	}
}

class TC2 extends Thread{
	public void run() {
//		for (int i = 1; i <= 1000; i++) {
//			System.out.println(i);
//		}
		
		System.out.println(Thread.currentThread().getName());
	}
}
```

# Answer for 4,5 and 6th questions in one code.
## 4. What is the join() method and when to use it
## 5. What is the main thread? 
## 6. What are synchronized blocks and methods and why do we need to use these in our code?
```java
public class SampleClass {
	public static void main(String[] args) throws InterruptedException {
		WebCount obj = new WebCount();
		
		Thread t1 = new Thread(new Runnable() {
			@Override
			public void run() {
				for (int i = 1; i <= 1000; i++) {

					obj.webcount();

				}
				System.out.println("Done with T1");

			}
		});
		Thread t2 = new Thread(new Runnable() {
			@Override
			public void run() {
				for (int i = 1; i <= 1000; i++) {

					obj.webcount();

				}
				System.out.println("Done with T2");
			}
		});
		t1.start();
		t2.start();

		// Waits for this thread to die. {Main thread will wait until this thread is finished}
		t1.join();

		// Waits for this thread to die. {Main thread will wait until this thread is finished}
		t1.join();

		System.out.println("Count: " + WebCount.count);
		System.out.println("Done with main work!");
	}
}

class WebCount {
	static int count = 0;// Shared resource

	synchronized void webcount() {
		count++;

		//synchronized(this){
		//	count++;
		//}
	}
}
```

## 7. What is Thread Life Cycle? What are the different states of the Thread Life Cycle?
![image](https://github.com/AkashChauhanSoftEngi/Java_Programming_Practice/assets/23252844/6d261abb-a7bb-444c-b91c-7ad733f64b8f)

> 1. New: When a thread lies in the new state, its code is yet to be run and such threads are waiting to be added in Thread Scheduler.
> 2. Runnable: Now the thread is added into a “Thread Scheduler” where threads wait for their turn to get the CPU.
> 3. Running: Now, the thread code (run method) gets the CPU 
> 4. Terminated: Dedicated work of a thread is done, now Thread Schedular has moved it to terminated state and that thread can never be invoked now.
> 5. Blocking/Waiting/Sleeping: Came out of “Running State”, and waiting for for some time or notify method to be called

## 8. What is Inter Thread Communication (ITC) in Java?
> 1. Inter-thread communication is a mechanism in which one thread is paused and another thread is allowed to enter the same critical section to be executed.
> 2. We have to use wait(), and notify() methods from the Object class in Java.

## 9. What is the Deposit and Withdraw problem (P1) that comes under Inter Thread Communication (ITC)?
![image](https://github.com/AkashChauhanSoftEngi/Java_Programming_Practice/assets/23252844/d4ae53f3-5888-4a89-9b02-f4192a22627c)

```java
public class SampleClass {

	public static void main(String[] args) {

		SharedResource c = new SharedResource();

		new Thread(new Runnable() {

			public void run() {
				c.withdraw(15000);
			}
		}).start();

		new Thread() {
			public void run() {
				c.deposit(10000);
			}
		}.start();
	}

}

class SharedResource {
	int amount = 10000;

	synchronized void withdraw(int amount) {
		if (this.amount < amount) {
			System.out.println("Less balance; waiting for deposit...");
			try {
				wait();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		int temp = this.amount - amount;

//		this.amount -= amount;

		if (temp < 0) {
			System.out.println("Negative Balance");
			try {
				wait();
			} catch (Exception e) {
			}
		}

		this.amount = temp;

		System.out.println("Now total amount is: " + this.amount);
		System.out.println("withdraw completed...");

	}

	synchronized void deposit(int amount) {
		this.amount += amount;
		System.out.println("deposit completed... ");
		System.out.println("Now total amount is: " + this.amount);
		notify();
	}
}

```

## 10. What is the Producer and Consumer problem (P2) that comes under Inter Thread Communication (ITC)?
> 1. The role of the producer involves creating data, placing it into the buffer, and then repeating the process. Simultaneously, the consumer is tasked with extracting the data from the buffer, one unit at a time.
> 2. To ensure the producer refrains from adding data when the buffer is full, and the consumer avoids attempting to remove data from an empty buffer.

![image](https://github.com/AkashChauhanSoftEngi/Java_Programming_Practice/assets/23252844/d45146dc-f857-4555-b3c2-b803a187595f)


```java
public class SampleClass {

	public static void main(String[] args) {

		Queue<Integer> buffer = new LinkedList<>();
		int maxSize = 10;

		Thread producer = new Producer(buffer, maxSize, "PRODUCER");
		Thread consumer = new Consumer(buffer, maxSize, "CONSUMER");

		producer.start();
		consumer.start();

	}

}

class Producer extends Thread {
	private Queue<Integer> queue;
	private int maxSize;

	public Producer(Queue<Integer> queue, int maxSize, String name) {
		super(name);
		this.queue = queue;
		this.maxSize = maxSize;
	}

	@Override
	public synchronized void run() {
		while (true) {
			synchronized (queue) {
				while (queue.size() == maxSize) {
					try {
						System.out.println("Queue is full, " + "Producer thread waiting for "
								+ "consumer to take something from queue");
						queue.wait();
					} catch (Exception ex) {
						ex.printStackTrace();
					}
				}
				Random random = new Random();
				int i = random.nextInt();
				System.out.println("Producing value : " + i);
				queue.add(i);
				queue.notify();
			}
		}

	}
}

class Consumer extends Thread {
	private Queue<Integer> queue;
	private int maxSize;

	public Consumer(Queue<Integer> queue, int maxSize, String name) {
		super(name);
		this.queue = queue;
		this.maxSize = maxSize;
	}

	@Override
	public void run() {
		while (true) {
			synchronized (queue) {
				while (queue.isEmpty()) {
					System.out.println("Queue is empty," + "Consumer thread is waiting"
							+ " for producer thread to put something in queue");
					try {
						queue.wait();
					} catch (Exception ex) {
						ex.printStackTrace();
					}
				}
				System.out.println("Consuming value : " + queue.remove());
				queue.notify();
			}
		}
	}
}
```
