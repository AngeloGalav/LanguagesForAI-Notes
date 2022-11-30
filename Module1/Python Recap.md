# Part 2
To import a library in python, or specific python functions:
```python
from math import pi, sin 
print(sin(pi))

import math 
print(math.sin(math.pi))
```

# Part 3
#### General stuff
- In Python, ==any value is an object (even int values)==.
- We may have distinct objects (with different identities) and same value.
- Identity of an object is completely controlled by the Python machine. We can see the identity of an object through the `id()` function. 

- to compare identity (and __NOT__ values) we use the operataro `is` . If two objects have the same identity, their value is also the same.

- The object receiving the method can be a constant, a name, an expression… (in summary: ==can be any Python expression evaluating to an object==)

## Tuple
- tuples are a structured type: its values are groups of values, __each of them of arbitrary type__.
- tuples are ==__immutable__== sequences of objects.
	- `()` is the empty tuple 
	- the tuple with a single element is `(<element>, )`.
	- Operations: concatenation (+), repetition (\*), length `len()`, selection `[…]`.

Consider this example:
```python
T = (1,2,3) 
T = T + (4,5) #Any operation creates a new object! 

V = (100,200) 
S = V # S and V are two names for the same object!
```

##### `tuple()` as a type converter
As any name of a type, `tuple()` may be used to convert values into tuples.
```python
tuple()           # -> ()
tuple('bolo')     # -> ('b','o','l','o')
tuple((1,2,3))    # -> (1,2,3)

tuple(100)        # -> error
tuple(range(2,6)) # -> (2,3,4,5)
```

Aliasing on tuple is _harmless_, because tuple are immutable.

### Generalised assignment
```python
A,B = (1,2) 
C, D, E = A+1, 5, B
C, E = C+1, E+1

A,B = B,A #this the “Pythonic” way of swapping two bindings
```

## Slicing
```python
s='Bologna b school' 
print(s[0]) # 'B' 
print(s[8:11]) # 'b s' 
print(s[:7]) # 'Bologna' 
print(s[10:]) # 'school'

s = "bologna BO"
s[0:4] #bolo
s[3:-1] #ogna B
s[:4] #bolo
s[3:] #ogna BO
s[:] #bologna BO
s[:-1] #bologna B

s[1:9:2] 
s[ : 100 : 4] 
s[3 : : 3] 
s[3 :-1 : 3]
s[5:2:-1] #ngo
```

Negativei step: is basically like saying:
```
from end of the sequence (last included!)
	to start (0 included! aka not included) 
(copy of) s reversed
```

## Range
```
range(<start>,<end>,<step>)
range(<end>) ---> shorthand for range(0,<end>,1)
```

## While
```
while <bool-expr>:
	<block>
```

### Lists
- structured type (like tuple) 
- __mutable__ sequences of objects
- Presentation: `[<object1>, … , <objectk>]`
- [] is the empty list

#### Methods
```python
L = [10,20,30,40]

L.pop([index])
L.remove([value])

L.append(x)
L.insert(pos, x)
L.extende(L1) # adds a list on the tail

L[:] #L.copy(), both are shallow copies
L.revese() # L[::-1] is a new object, this method reverse() is instead in place.
L.sort() # modifies the whole object (in place modification)

```

#### Assigments in Lists
\<selection\> = \<expression\>
\<selection\> may be a slice
If `<selection>` is a slice:
- RHS must be a list, whose elements are inserted for the slice
- as a result of the assignment, the list at LHS may grow, or shrink 

Example:
```Python
del L[i:j] #is equivalent to L[i:j]=[]
L[1:2]=[4,5,6] # EXTENDS THE LIST
L[1]=[4,5,6] # ADDS THE LIST AS AN OBJECT
```

> [!warning]
> `L = L + [el]` != `L.append(el)`
> The first creates a new object, the second __modifies__ the list directly. 

Example:
```python
L=[10,20,30] 
W=L 
L.append(40) 
print(W) 
L=L+[60] 
print(W)

# prints [10,20,30,40] in both cases
```

Example 2:
```Python
L=[10] 
for e in L: 
	L.append(e) 
print(L)
	## !!! INFINITE LOOP

L=[10] 
for e in L: 
	L=L+[e] 
print(L)

## adds a single element

```

##### The += operator
The `+=` operator modifies the object, while `+` creates a new one:
```python
L = [1,2,3] 
L += [4] 

V = [1,2,3] 
V = V + [4]
```

Also, `+=` evaluates the any operator only once:
```Python
L[f(i)] += val 
L[f(i)] = L[f(i)] + val # f(i) is called twice
```
Also, LHS is evaluated before RHS. 


### Global declaraction
```Python
a = 1
b=20 
def f(a): 
	global b # b now refers to the global b, and is not a new variable decla 
	a=10 # new name 
	b=100 
	return None 
f(b)
print(b)
```

## Comprehension
```Python
doubles=[]
for i in range(0,101): 
	doubles.append(2*i)

## is equal to
doubles= [2*i for i in range(0,101)]
```

```Python
even=[] 
for i in range(0,101): 
	if i%2==0: 
		even.append(i) 
#is equal to		
even=[i for i in range(0,101) if i%2==0]
```

#### Other example
```Python
LL = [ [k for k in range(i)] for i in range(20)]
H = [(x,y) for x in range(2) for y in range(3)]
```
Essentially:
A more complex version
```
[expression for name in sequence series of for/if ] 
```

equivalent to 
```Python
res=[] 
for name in sequence : 
	series of for/if 
	res.append(expression) 
return res
```

### Dictionaries
```Python
L = {}
L = {1:2, 3:'suca', 'vaff':'3'}
len(L)
L['suca']
del L['suca']
L['nuovo'] = 8 # nuovo oggetto nel dizionarion

D1==D2 # True sse D1 and D2 have the same items, independently from the order of the items

D1.update(D2) # similar to concatenation, but if we have similar case, d2 value is chosen

list(D1) # returns a list of tuples

zip(L1, L2) # creates a list of tuples (L1, L2), easily convertible to a dict
```

### nonlocal command
Similar to global. references the upper scope (not global)

- `globals()` returns the dict of the global names with their binding
- `locals()` returns the dict of the local names with their binding

### Lambda expressions
`lambda z:z+1` is an expression denoting an (anonymous) function, and is the same as 

```Python
def succ(z): 
	return z+1
```

#### Comprehension on dicts
{key:val for name in sequence}
```
{i:i**2 for i in range(10) if i%2==0}
{i:[k for k in range(i)] for i in range(10)}
```

#### Generator
They are not tuples, and are defined like comprenshions.
Through the function `next()`, we can access the successor of the generated series. 

They are useful in iterations. 

They are like defining function using the `yield` keyword. 

# Classes & Objects

In python, classes' attributes are defined using the `__init(self)__` method inside the class.
`self` is always the first parameter of the method. 

`__init()__` gets called when we call `Class()`.

### add() method
`__str__()` method
`__eq__()` method
`__ne__()`
`__len__()` for len
`__getitem__()` for self\[key\]

## Private attributes
`__name()` is a private attribute

## class_attribute (or static)
```Python 
class A: 
	class_attr = 10 ### this class attribute's value is shared by all objects (and so are its modification)
	def __init__(self,par): 
		self.a = par 
	def inc(self): 
		self.a += A.class_attr 
	def __str__(self): 
		return 'A('+str(self.a)+')'
```


## Subclasses in Python
```Python
class Point :
	...

class NewPoint (Point): 
	def origin(self): 
		self.x,self.y=0,0
```

##### method super()
super() references the superclass method.
```Python
class ColPoint(Point): 
	def __init__(self,xx,yy,col): 
		super().__init__(xx,yy) 
		self.c=col 
	def origin(self): 
		self.x,self.y=0,0
```

## Dynamic method lookup
![[dynamic_method_lookup.png]]

### Abstract classes 
```Python
def MyAbstract(ABC): 
@abstractmethod 
def method1(self): 
	pass 
@abstractmethod 
def method2(self): 
	pass
```

## Iterators
``` Python
class MyIter: #we iterate on even int in an interval 
def __init__(self,start,stop): 
	if start%2!=0: 
		start+=1 
	self.x=start s
	elf.stop=stop 

def __iter__(self): # return the iterator 
	return self 

def __next__(self): # return the "next" object 
	if self.x < self.stop: 
		curr=self.x 
		self.x+=2 
		return curr
	else: 
		raise StopIteration
```

### try/except command
```
try:
	<block>
except <exception>:
	<handlerblock>
<after>

try:
	<block>
except : # catches all exceptions
	<handlerblock>
<after>

```

Example 
```Python
class MyExcp(Exception): 
	pass

P=(MyExcp, IndexError) 

try: 
	raise P[0] 
except MyExcp: 
	print("caught")
```

A clause
`except <exceptionclass>`
will catch all exceptions which are instances of \<exceptionclass\>
(therefore all instances of its subclasses)


# NumPy
- ndarray
	- fixed data (if changed, the older array gets copied and deleted)
	- homogenous (all data in it is of hte same type, whihc could also be 'python object')![[vectorization.png]]
![[vec2.png]]
#### Creating arrays
```Python
np.zeros((3,2))# the arg must be a shape return a zero matrix, dtype=float64 
np.ones((3,2)) #return an all-one matrix, dtype=float64 
np.empty((3,2)) #return an uninitialized matrix, dtype=float64


np.arange(10,30,5).reshape((2,2)) # array([[10, 15], [20, 25]])
# SHAPE MUST MATHC THE SIZE OF the array

np.linspace(0,2,9) # array([0., 0.25, 0.5 , 0.75, 1., 1.25, 1.5 , 1.75, 2.])

x=np.array([[1,2,3,4],[5,6,7,8]])
x[0,2] # to access a 2-array
```

![[ndarray.png]]![[Screenshot_20221117_030030.png]]