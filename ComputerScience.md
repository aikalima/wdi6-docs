

Intro do Algorithms
--


Part 1 - start with those slides

http://courses.cs.vt.edu/~csonline/Algorithms/Lessons/Introduction/index.html

Put alpha in order … describe the algo that you used.

Page2:
- Structured English!


Sorting
--

Back to other presentation:
http://courses.cs.vt.edu/~csonline/Algorithms/Lessons/SortingAlgorithms/index.html

- Simple Sort
- Insertion Sort
- Selection Sort

Big-O
--
**Now jump to Big 0**

Start here Slide 3 to 6:
http://www.slideshare.net/multimedia9/sorting-algorithms-presentation

**Think of complexity as number of steps, relative to the input size**

**describe the performance or complexity of an algorithm. Big O specifically describes the worst-case scenario, and can be used to describe the execution time required or the space used (e.g. in memory or on disk) by an algorithm.**

Use example on page 4

Then here - plain english:
http://stackoverflow.com/questions/487258/plain-english-explanation-of-big-o



###Other examples:

**O(1)** describes an algorithm that will always execute in the same time (or space) regardless of the size of the input data set.

```
a[0]
a.length? Maybe.
puts "hello"
```

**O(n)** describes an algorithm whose performance will grow linearly and in direct proportion to the size of the input data set. 


```
"ABCDEFG".split("").each do |i|
  puts i
end
```

Good example: string contain

```
unless "contains" of String::
  String::contains = (str, startIndex) ->
    -1 isnt String::indexOf.call(this, str, startIndex)
    
if (!("contains" in String.prototype)) {
  String.prototype.contains = function(str, startIndex) {
    return -1 !== String.prototype.indexOf.call(this, str, startIndex);
  };
}

```
    
**O(N2)** represents an algorithm whose performance is directly proportional to the square of the size of the input data set. This is common with algorithms that involve nested iterations over the data set. Deeper nested iterations will result in O(N3), O(N4) etc.    
    
Example: duplicates:

```
containsDuplicates(strings) 
{
	for(int i = 0; i < strings.Length; i++)
	{
		for(int j = 0; j < strings.Length; j++)
		{
			if(i == j) // Don't compare with self
			{
				continue;
			}

			if(strings[i] == strings[j])
			{
				return true;
			}
		}
	}
	return false;
}
```


**O(2 to power of N)** denotes an algorithm whose growth will double with each additional element in the input data set. The execution time of an O(2N) function will quickly become very large.


**O(log n)**

Wikipedia: **The logarithm of a number is the exponent to which another fixed value, the base, must be raised to produce that number. For example, the logarithm of 1000 to base 10 is 3, because 1000 is 10 to the power 3: 1000 = 10 × 10 × 10 = 103. More generally, if x = by, then y is the logarithm of x to base b, and is written y = logb(x), so log10(1000) = 3.**

Example: Finding a value in a soretd array:
What's the algo like and why is it O(log n)?

**Variations**

O(n log n)

http://bigocheatsheet.com/

Comparison and swap operations - remember tower of hanoi …

**Simple Sort .. isn't that what you did when sorting chars in the bginning?**

What's the complexity? -> O(n * n-1) 


http://courses.cs.vt.edu/~csonline/Algorithms/Lessons/SpaceEfficiency/index.html 
14
8
8

More efficient algorithms …

http://www.sorting-algorithms.com/

Cool - explore …


New data structure … Binary Tree
--

Back to the Curriculum ..

https://docs.google.com/document/d/1IYRBm_QwjlqLNt6q2_p43dd_7qxy2YZImgrYA59TToI/edit

In context of logarithms ..

- intro
- Code along


/Code/binary_tree

```
require 'pry'
require 'pry-debugger'
require 'pry-stack_explorer'

class Node
  attr_accessor :d, :p, :n

  def initialize(data)
    @d = data
    @p = @n = nil
  end

  def to_s
    nxt = @n.nil? ? 'empty' : @n.d
    prv = @p.nil? ? 'empty' : @p.d
    "#{prv} <- #{@d} -> #{nxt}"
  end
end

#binding.pry
class Tree
  attr_accessor :root, :nodes

  def insert(name)
    if @root.nil?
      @root = Node.new(name)
    else
      insert_node(@root, name)
    end
  end

  def insert_node(node, name)
    if (name < node.d) && (!node.p.nil?)
      insert_node(node.p, name)
    elsif (name > node.d) && (!node.n.nil?)
      insert_node(node.n, name)
    elsif (name < node.d) && (node.p.nil?)
      node.p = Node.new(name)
    elsif (name > node.d) && (node.n.nil?)
      node.n = Node.new(name)
    end
  end

  def find(name)
    find_node(@root, name)
  end

  def find_node(node, name)
    if name == node.d
      node
    elsif name < node.d && !node.p.nil?
      find_node(node.p, name)
    elsif name > node.d && !node.n.nil?
      find_node(node.n, name)
    else
      nil
    end
  end

  def to_s
    @nodes = []
    inorder_traversal(@root)
    #preorder_traversal(@root)
    #postorder_traversal(@root)
    @nodes.join(', ')
  end

  def inorder_traversal(node)
    inorder_traversal(node.p) if !node.p.nil?
    @nodes << node.d
    inorder_traversal(node.n) if !node.n.nil?
  end

  def preorder_traversal(node)
    @nodes << node.d
    preorder_traversal(node.p) if !node.p.nil?
    preorder_traversal(node.n) if !node.n.nil?
  end

  def postorder_traversal(node)
    postorder_traversal(node.p) if !node.p.nil?
    postorder_traversal(node.n) if !node.n.nil?
    @nodes << node.d
  end

end

t1 = Tree.new
t1.insert('Josh')
t1.insert('Karl')
t1.insert('Justin')
t1.insert('Ian')
t1.insert('Matt')
t1.insert('David')
t1.insert('Xiao')
t1.insert('Donielle')
t1.insert('Lior')
t1.insert('Bryan')
t1.insert('Ryan')
t1.insert('Lauren')
t1.insert('Sharif')
t1.insert('Ricky')
t1.insert('Alex')
t1.insert('Todd')


brian = t1.find('Bryan')
ricky = t1.find('Ricky')
karl = t1.find('Karl')
puts "K: #{brian}"
puts "L: #{ricky}"
puts "F: #{karl}"
puts "T: #{t1}"



```

Binary search is a technique used to search sorted data sets. It works by selecting the middle element of the data set, essentially the median, and compares it against a target value. If the values match it will return success. If the target value is higher than the value of the probe element it will take the upper half of the data set and perform the same operation against it. Likewise, if the target value is lower than the value of the probe element it will perform the operation against the lower half. It will continue to halve the data set with each iteration until the value has been found or until it can no longer split the data set.
This type of algorithm is described as O(log N). The iterative halving of data sets described in the binary search example produces a growth curve that peaks at the beginning and slowly flattens out as the size of the data sets increase e.g. **an input data set containing 10 items takes one second to complete, a data set containing 100 items takes two seconds, and a data set containing 1000 items will take three seconds.** Doubling the size of the input data set has little effect on its growth as after a single iteration of the algorithm the data set will be halved and therefore on a par with an input data set half the size. This makes algorithms like binary search extremely efficient when dealing with large data sets.


**W9D3 Big O diagram array vs hashes - short discussion of data strcuture**


Sorting
--

http://en.wikipedia.org/wiki/Sorting_algorithm

Bubble Sort animation:
https://cs.senecac.on.ca/~catherine.leung/sketches/bubble.html

http://www.sorting-algorithms.com/quick-sort


Binary Trees
--

W9D4 … good example - walk through it …
Code/binary_tree.rb

http://en.wikipedia.org/wiki/Tree_traversal
 



