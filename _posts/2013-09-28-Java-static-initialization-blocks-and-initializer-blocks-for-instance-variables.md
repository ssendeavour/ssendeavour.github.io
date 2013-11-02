---
layout: post
title: Java static initialization blocks and Initializer blocks for instance variables
date: Sat Sep 28 02:47:33 CST 2013
categories: Java
---

This tutorial is acquired from [javadoc].

## Static Initialization Blocks

A class can have any number of static initialization blocks, and they can appear anywhere in the class body. The runtime system guarantees that static initialization blocks are called in the order that they appear in the source code.

## Initializing Instance Members

Normally, you would put code to initialize an instance variable in a constructor. There are two alternatives to using a constructor to initialize instance variables: initializer blocks and final methods.
	
The Java compiler copies initializer blocks into every constructor. Therefore, this approach can be used to share a block of code between multiple constructors.


## Example:

~~~
public class TestMain {
 
	public static void main(String[] args) {
		System.out.println(new Point());
		System.out.println(new Point(30));
		System.out.println(new Point(30, 20));
		System.out.println(Point.count);
		System.out.println(Point.name);
	}
 
	class Point {
		public static int count;
		public static String name;
	 
		private int width, height;
		private int foo;
		private String bar;
	 
		// initialize class variables 
		static {
			count = 112;
			name = "Point Name";
		}
	 
		// common initialization block, run with every constructor and run before them
		{
			width = height = 10;
			foo = 23;
			bar = "barString";
		}
	 
		public Point() {
		}
	 
		public Point(int x) {
			width = height = x;
		}
	 
		@Override
		public String toString() {
			return "Point [width=" + width + ", height=" + height + ", foo=" + foo + ", bar=" + bar + "]";
		}
	 
		public Point(int w, int h) {
			width = w;
			height = h;
		}
	}
}
~~~

the output:
	
~~~
Point [width=10, height=10, foo=23, bar=barString]
Point [width=30, height=30, foo=23, bar=barString]
Point [width=30, height=20, foo=23, bar=barString]
112
Point Name
~~~

[javadoc]: http://docs.oracle.com/javase/tutorial/java/javaOO/initial.html
