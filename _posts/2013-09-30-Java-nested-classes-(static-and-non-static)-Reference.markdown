---
layout: post
title:  "Java Nested Classes(static and non-static), local class, and anonymous class Reference"
date:   2013-09-28 02:12:24
categories: Java
---
Java doc has a very good documents about this. I think Java docs is the best place to learn java, way better than some textbooks. So I only place a link to there to let you know this good resource.

[http://docs.oracle.com/javase/tutorial/java/javaOO/nested.html](http://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)

# summary

 Nested classes are divided into two categories:static and non-static, can be decalared private, public, protected, or package private:
 
## static nested classes
Nested classes that are declared static. Like static method, a static nested class cannot refer directly to instance variables or methods defined in its enclosing class. It can use them only through an object reference.

In effect, a static nested class is behaviorally a top-level class that has been nested in another top-level class for packaging convenience.

## inner classes
Non-static nested classes. As with instance methods and variables, an inner class is associated with an instance of its enclosing class and has direct access to that object's methods and fields. Also, because an inner class is associated with an instance, it cannot define any `static` members itself.

To instantiate an inner class, you must first instantiate the outer class. Then, create the inner object within the outer object with this syntax:

{% highlight java %}
OuterClass.InnerClass innerObject = outerObject.new InnerClass();
{% endhighlight %}

There are two special kinds of inner classes: [local classes](http://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html) and [anonymous classes](http://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html).

### Local classes
Local classes are classes that are defined in a block, which is a group of zero or more statements between balanced braces. You typically find local classes defined in the body of a method. ( see original document for an example)

- A local class has access to the members of its enclosing class
- A local class has access to local variable that are declared``fina`.
- Starting in Java SE 8, a local class can access local variables and parameters of the enclosing block that are `final` or effectively final. A variable or parameter whose value is never changed after it is initialized is effectively final. 
- Starting in Java SE 8, if you declare the local class in a method, it can access the method's parameters
- You cannot declare static initializers or member interfaces in a local class.(in my comprehention, you cannot declare static method in local class)

### Anonymous classes
Anonymous classes enable you to make your code more concise. They enable you to declare and instantiate a class at the same time. They are like local classes except that they do not have a name. Use them if you need to use a local class only once.

#### Syntax of Anonymous Classes
While local classes are class declarations, anonymous classes are expressions. Consider the instantiation of the frenchGreeting object:

{% highlight java %}
HelloWorld frenchGreeting = new HelloWorld() {
	String name = "tout le monde";
	public void greet() {
		greetSomeone("tout le monde");
	}
	public void greetSomeone(String someone) {
		name = someone;
		System.out.println("Salut " + name);
	}
};
{% endhighlight %}

The anonymous class expression consists of the following:

* The new operator
* The name of an interface to implement or a class to extend. In this example, the anonymous class is implementing the interface HelloWorld.
* Parentheses that contain the arguments to a constructor, just like a normal class instance creation expression. Note: When you implement an interface, there is no constructor, so you use an empty pair of parentheses, as in this example.
* A body, which is a class declaration body. More specifically, in the body, method declarations are allowed but statements are not.

Anonymous classes have the same access to local variables of the enclosing scope as local classes.

Anonymous classes are often used in graphical user interface (GUI) applications. For example:

{% highlight java %}
btn.setOnAction(new EventHandler<ActionEvent>() {
 
	@Override
	public void handle(ActionEvent event) {
		System.out.println("Hello World!");
	}
});
{% endhighlight %}


