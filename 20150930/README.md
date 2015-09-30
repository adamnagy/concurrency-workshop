Concurrency Exercises
=====================

git clone git@github.com:peterborkuti/concurrency-workshop.git

Hints: 
* If you are stuck, ask your neighbour

* If you are really stuck, ask me

* If you are really-really stuck, check this repo: https://github.com/peterborkuti/concurrency



Author: Péter Borkuti

Creating threads
----------------
0. clone the repository and check the _Util_ class

1. Extend Thread
   thread must write "Start", wait for a second and write "Done".
   Hint: http://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html first part

2. Implement Runnable
   runnable must write "Start", wait for a second and write "Done".
   Hint: http://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html second part

3. Implement Runnable and use executor (_executor01/Main.java_)
  runnable must write "Start", wait for a second and write "Done".
  ```
  ExecutorService executor = Executors.newFixedThreadPool(8);
  ...
  executor.execute(sleeper);
  Util.sleep(5000,1000);
  Util.shutdownAndAwaitTermination(executor);
  ```

4. Implement runnable, run 10 pieces of them with executor (_executor02/Main01.java_)

Let's forget extending Thread!
------------------------------------

1. Thinker01 (_executor02/Main02.java_)
Create Thinker01 which chooses a random number after a random amount of thinking
and writes this number to the output! Use executor.

2. Modify it to use Future! (_executor02/Main03.java_)
  ```
  Future<?> future = executor.submit(new Thinker01(), null);
  future.get();
  ```
3. Thinker02 (_executor02/Main04.java_)
  Create Thinker02 which implements Callable and returns with the choosen number:
  ```
  Future<Integer> future = executor.submit(new Thinker02());
  Integer output = future.get();
  ```
4. Thinkers
  Create 10 pieces of Thinker02, execute them and write the sum of them outputs!

5. Thinkers and executor.invokeAll (_executor04/Main.java_)
  The same as above, but create a List of Callable, add the Thinker02 objects to it,
  use executor.invokeAll(callables). This will return a List of Future objects.
  Use this list to generate the sum of the outputs!
  ```
  List<Callable<Integer>> callables = new ArrayList<Callable<Integer>>();
  ...
  [add new Thinker02()'s to callables]
  ...
  List<Future<Integer>> futures = executor.invokeAll(callables);
  ...
  ```

Let's use latch!
----------------

1. Create two threads. The second should wait the first to finish
2. Create N threads and start them. Create a thread Q which should wait for the other N to finish.
3. Create N threads. They should wait for each other to start processing.

Other exercises
---------------

1. Gardener
  seeds, gardener. Seeds must be watered to grow. After they reach a certain size, harvested.

2. Dishwashers
  See in the book

3. Authors
  5 authors: sleeping, waiting for a pen, writing. 3 pens.

4. Automatic Traffic Light
  Light: red. Green for a given time if there is a car in front of it.

5. Two dependent automatic traffic light
  Green for a given time if there is a car in front of it and the other light is not green

6. Elevator1
  Two stops (Up and Down). People waiting at the stops.

7. Elevator2
  N stops. People waiting at the stops and when get in, choose a random Stop.
