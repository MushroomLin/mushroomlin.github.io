---
title:  "What methods are in Java Object class?"
date:   2019-10-08 00:00:00
categories: [Java]
tags: [Java]
---

In Java, Object class is the base class for any classes. According to the official document of [Java][https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html], the Object class have 11 methods:

1. **public final Class<?> getClass()**: Returns the runtime class of this `Object`. The returned `Class` object is the object that is locked by `static synchronized` methods of the represented class.
2. **public int hashCode()**: Returns a hash code value for the object. This method is supported for the benefit of hash tables such as those provided by [`HashMap`](https://docs.oracle.com/javase/7/docs/api/java/util/HashMap.html). By default, it converts the address of the object into a integer.
3. **public boolean equals(Object obj)**: Indicates whether some other object is "equal to" this one.
4. **protected Object clone() throws CloneNotSupportedException:** Creates and returns a copy of this object. The precise meaning of "copy" may depend on the class of the object.
5. **public String toString():** Returns a string representation of the object. In general, the `toString` method returns a string that "textually represents" this object. The result should be a concise but informative representation that is easy for a person to read. It is recommended that all subclasses override this method.
6. **public final void notify():** Wakes up a single thread that is waiting on this object's monitor. If any threads are waiting on this object, one of them is chosen to be awakened. The choice is arbitrary and occurs at the discretion of the implementation. A thread waits on an object's monitor by calling one of the `wait` methods.
7. **public final void notifyAll():** Wakes up all threads that are waiting on this object's monitor. A thread waits on an object's monitor by calling one of the `wait` methods.
8. **public final void wait(long timeout) throws InterruptedException:** Causes the current thread to wait until either another thread invokes the [`notify()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#notify()) method or the [`notifyAll()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#notifyAll()) method for this object, or a specified amount of time has elapsed.
9. **public final void wait(long timeout, int nanos) throws InterruptedException**: Causes the current thread to wait until another thread invokes the [`notify()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#notify()) method or the [`notifyAll()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#notifyAll()) method for this object, or some other thread interrupts the current thread, or a certain amount of real time has elapsed.
10. **public final void wait() throws InterruptedException**: Causes the current thread to wait until another thread invokes the [`notify()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#notify()) method or the [`notifyAll()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#notifyAll()) method for this object. In other words, this method behaves exactly as if it simply performs the call `wait(0)`.
11. **protected void finalize() throws Throwable**: Called by the garbage collector on an object when garbage collection determines that there are no more references to the object. A subclass overrides the `finalize` method to dispose of system resources or to perform other cleanup.

