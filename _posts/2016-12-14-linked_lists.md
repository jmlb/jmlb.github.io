---
layout: post
title:  Data Structures - Linked lists (Part 1/3)
tags: Datastructures
category: coding
author: "Jean-Marc Beaujour"
summary: "Linked list implemented in Python"
category: coding
img_post: datastructures.jpg
github-link: na
---

This is the first part of a series of posts on **DataStructures**. The content of the posts is mainly from the "Technical Interview Practice" module in the Machine Learning Nanodegree (Udacity). I will include also the Quizzes (and their answers :-) ). The Quizzes were actually pretty good: proposing good examples for the implementation of the materials covered in the videos.



1. Linked lists
2. [Stacks]({% post_url 2016-12-15-stacks %})
3. [Queues]({% post_url 2016-12-16-queues %})



Before we talk about Datastructures, we need to talk about code efficiency. The efficiency / Complexity of a code is estimated in term of:

+ **space**-complexity: how much space the code requires, and 

+ **time**-complexity: how long the code takes to run

Those terms are typically estimated as a function of the length *n* of the input. The complexity is expressed as * O(n...)* (pronouce *big-O*)

Typically, we look at the worst case scenario when evaluating the complexity. 


Ok! Now that we've got that out of the way, we will dive in and talk about our first data structure: **linked lists**.


### Linked lists
<hr>
A Linked list is not an array.

An array is a list with the following rules:

+ each array element has an index (the element can be only numbers, characters, strings, or a mix):

<img src="/pics/posts_pics/datastructures/array_pt1.png" height="75px">

+ a drawback of arrays is that it is difficult to insert or delete elements

<img src="/pics/posts_pics/datastructures/linkedlist-array_p1.png" height="200px">


A linked list must have a **head** element, i.e the first element in the list. Each element only knows about the next element but that is all (**next** is a built-in iterator). Adding and removing element from a linked list is easier than for an array.

<img src="/pics/posts_pics/datastructures/linkedlist-array2_p1.png" height="200px">


In the linked list, we store :

+ the *value*: it can be a number or a character

+ a reference to the next element in the list of the element

The advantage of Linked list is that removing and inserting elements can be done in constant time.



### Double linked lists
<hr>
**Double linked lists** use pointers to the next element and to the previous element.

<img src="/pics/posts_pics/datastructures/double_linkedlist.png" height="200px">



### Quizz
<hr>
Add/Complete the code for the three functions to the LinkedList:

+ `get_position()` returns the element at a certain position.
+ `insert()` function will add an element to a particular spot in the list.
+ `delete()` will delete the first element with that particular value.


{% highlight python linenos%}
class Element(object):
    def __init__(self, value):
        self.value = value #provide the value of the element
        self.next = None #is a pointer to the next elemetn (memory location)
         
class LinkedList(object):
    def __init__(self, head=None):
        self.head = head
        

     #if the linkedlist has a head, iterate thru next reference in every elemnt until you reach the end of the list do nothing

    def append(self, new_element):
        current = self.head
        if self.head:
            while current.next:
                current = current.next
            current.next = new_element
        else:
            self.head = new_element
            
    def get_position(self, position):
        """Get an element from a particular position."""
        i = 1
        current = self.head

        if position < 1:
        	return None

        while current and counter <= position:
            if i == position:
                return current
            current = current.next
            i += 1
        return None
    
    def insert(self, new_element, position):
        """Insert a new node at the given position.
        Assume the first position is "1".
        Inserting at position 3 means between
        the 2nd and 3rd elements."""
        if position == 1:
        	new_element.next = self.head
        	self.head = new_element
        else:
        	prev = self.get_position(position - 1)
        	next_element = prev.next
        	#assign new connections
        	prev.next = new_element
        	new_element.next = next_element
    
    
    def delete(self, value):
        """Delete the first node with a given value."""
        current = self.head
        i = 1
        while current.value != value and current.next:
            current = current.next
            i += 1
        if current == self.head:
            self.head = current.next
        else:
            prev = get_position(i)
            prev.next = current.next.next
        
# Test cases
# Set up some Elements
e1 = Element(1)
e2 = Element(2)
e3 = Element(3)
e4 = Element(4)

# Start setting up a LinkedList
ll = LinkedList(e1)
ll.append(e2)
ll.append(e3)

# Test get_position
# Should print 3
print ll.head.next.next.value
# Should also print 3
print ll.get_position(3).value

# Test insert
ll.insert(e4,3)
# Should print 4 now
print ll.get_position(3).value

# Test delete
ll.delete(1)
# Should print 2 now
print ll.get_position(1).value
# Should print 4 now
print ll.get_position(2).value
# Should print 3 now
print ll.get_position(3).value
{% endhighlight %}

