# 集合类型

Swift提供三种基本集合类型，被称为数组，无序集合(Set)和字典，来储存值的集合。数组是有序的值集合。Set是无序且不重合的值的集合。字典是无序的键值对。

<p align="center">
<img src="https://docs.swift.org/swift-book/_images/CollectionTypes_intro_2x.png" alt="集合类型" width="600"/>
</p>

Swift中的数组，无序集合和字典总是知道它们所储存的值的类型。这意味着你无法无意插入错误类型的值到集合中。也意味着你能对从集合中拿到的值的类型充满自信。
> 注意：
Swift的数组，无序集合和字典类型实现来范型。更多关于范型，请看[范型](Generics.md)。

## 集合的可变性

如果你创建数组，无序集合或字典，然后赋值给变量，则创建的结合将是 *可变的(mutable)*。这意味着在集合被创建之后，你可以通过添加，移除或改变集合中的元素来修改集合。如果你赋值给常量，则该集合是 *不可变的(immutable)*，内容和大小都不能被你改变。

> 注意：
在集合不需要改变的情况下，最好创建不可变的集合。这样做可以使你更容易敲定代码，并使Swift编译器能帮你优化你所创建的集合的性能。

### 数组

数组用一个有序序列来储存相同类型的值。同一个值可以在一个数组中的不同地方出现若干次：
> 注意：
Swift的数组类型桥接于Foundation框架的`NSArray`类。
关于更多与Foundation和Cocoa框架一起使用`Array`的信息，参见[桥接Array与NSArray](https://developer.apple.com/documentation/swift/array#2846730)

#### 数组类型速记语法

Swift数组类型完整写法是`Array<Element>`，其中`Element`是数组可以储存的值的类型。你也可以用速记语法`[Element]`写数组的类型。虽然这两种写法在功能上相同，当提到数组的类型时，速记语法是首选，并被广泛使用在本指南中。

#### 创建空数组

你可以用初始化语法创建某一类型的空数组：
```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// Prints "someInts is of type [Int] with 0 items."
```

注意，变量`someInts`的类型可以从初始化起的类型推导出为`[Int]`。

另外，如果上下文已经提供了类型信息，例如函数的实参或已经有类型的常量或变量，你可以直接用空数组字面量创建一个空数组，写作`[]`(一对空的中括号)：
```swift
someInts.append(3)
// someInts now contains 1 value of type Int
someInts = []
// someInts is now an empty array, but is still of type [Int]
```

#### 用默认值创建数组

Swift提供创建将所有元素设定为同一个值的固定大小的数组的初始化方法。向此初始化方法传入适当类型的值（称为`repeating`），和该值在数组中重复的次数（称为`count`）：
```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```

#### 通过两个数组相加创建新数组

可以通过加号运算符将两个含有兼容类型值的数组相加的方式创建新数组。新数组的类型通过被相加的两个数组类型推导而来：
```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5]

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

#### 用数组字面量创建数组

也可以用数组字面量来初始化数组，这是创建含有一个或多个值数组的快捷方法。数组字面量被写作一个值列表，用逗号分隔，用中括号括起来：[`value1`, `value2`, `value3`]

下面的例子中，创建一个`shoppingList`数组用来存放`String`值：
```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList has been initialized with two initial items
```

变量`shoppingList`被声明为“一个字符串数组”，写作`[String]`。因为这个数组被指定来元素类型`String`，所以它指被允许用来存放字符串。在这里，数组`shoppingList`被两个字符串初始化，这两个字符串被写成数组字面量。
> 注意：
> `shoppingList`被声明为变量（使用了`var`关键字）而不是常量（`let`关键字），是因为接下来的例子中，会向其中添加更多的元素。

在这种情况下，数组字面量仅仅包含两个字符串。这满足了`shoppingList`变量所声明的类型（一个只能存放字符串的数组），所以数组字面量的赋值被允许用来初始化`shoppingList`。

这要感谢Swift的类型推导，如果你用含有相同类型元素的数组字面量初始化数组，你并不需要写上数组的类型。（译者：不要这么骄傲，很多语言都有类型推导的功能哦）`shoppingList`的初始化可以写得更简洁：
```swift
var shopping1 = ["Eggs", "Milk"]
```

因为字面量中所有值是相同类型，Swift可以推导出`[String]`是`shoppingList`的正确类型。

#### 访问和修改数组

可以通过数组的方法和属性或下标语法来访问和修改数组。

要获取数组中元素的个数，用只读属性`count`：
```swift
print("The shopping list contains \(shoppingList.count) items.")
// Prints "The shopping list contains 2 items."
```

用布尔属性`isEmpty`作为检查`count`属性是否为`0`等快捷方法：
```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// Prints "The shopping list is not empty."
```

可以通过调用`append(_:)`方法向数组末尾添加新元素：
```swift
shoppingList.append("Flour")
// shoppingList now contains 3 items, and someone is making pancakes
```

另外，用加法赋值符号(`+=`)向数组末尾添加含有一个或多个兼容元素的数组：
```swift
shoppingList += ["Baking Powder"]
// shoppingList now contains 4 items
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList now contains 7 items
```

用*下标语法*从数组中获取一个值，在数组名后面写上用方括号括起的你想获取的值的索引：
```swift
var firstItem = shoppingList[0]
// firstItem is equal to "Eggs"
```
> 注意：
> 数组中的第一个值索引为`0`，而不是`1`。Swift中的数组索引从零开始。

可以用下标语法修改指定索引对应的已经存在的值：
```swift
shoppingList[0] = "Six eggs"
// the first item in the list is now equal to "Six eggs" rather than "Eggs"
```

当使用下标语法时，你所以定的索引必须有效。例如，`shoppingList[shoppingList.count] = "Salt"`试图向数组的末尾添加元素，将导致运行时错误。

也可以用下标语法一次性改变一段范围的值，即使用来替换范围与被替换的范围长度不同。下面的例子将`Chocolate Spread`，`Cheese`和`Butter`替换为`Bananas`和`Apples`：
```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList now contains 6 items
```

要将一个元素插入到指定索引位置，调用`insert(_:at:)`方法：
```swift
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList now contains 7 items
// "Maple Syrup" is now the first item in the list
```

这里调用`insert(_:at:)`方法将一个新元素"Maple Syrup" 插入到购物清单数组的最前面，也就是索引为`0`的位置。

类似地，用`remove(at:)`方法移除一个元素。这个方法移除指定索引的元素然后返回该元素（如果不需要该返回值，可以忽略）：
```swift
let mapleSyrup = shoppingList.remove(at: 0)
// the item that was at index 0 has just been removed
// shoppingList now contains 6 items, and no Maple Syrup
// the mapleSyrup constant is now equal to the removed "Maple Syrup" string
```

> 注意：
> 如果你试图通过超出数组已有边界的索引来访问或修改数组的元素，将触发运行时错误。可以通过比较索引与`count`属性来检查该索引是够合法。因为数组的索引从零开始，所以最大的索引值是`count - 1`，但，当`count`是`0`时（数组为空），没有合法的索引。

移除元素时，数组中的间隔或被删除，如下，`Six eggs`再一次变成了索引为`0`的值：
```swift
firstItem = shoppingList[0]
// firstItem is now equal to "Six eggs"
```

如果要从数组中移除最后一个元素，用`removeLast()`方法而不是`remove(at:)`方法，以避免需要访问数组的`count`属性。像`remove(at:)`方法样，`removeLast()`方法也返回被移除的元素：
```swift
let apples = shoppingList.removeLast()
// the last item in the array has just been removed
// shoppingList now contains 5 items, and no apples
// the apples constant is now equal to the removed "Apples" string
```

#### 遍历数组

可以用`for-in`循环遍历数组的所有值：
```swift
for item in shoppingList {
  print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

 如果需要整数索引和值，用`enumerated()`方法遍历数组。对于数组中的每个元素，`enumerated()`方法返回一个整数与元素符合而成的元组。整数从零开始，每个元素加1；如果列举整个数组，这些整数将能匹配元素的索引。可以将元组拆解成临时常量或变量，作为遍历的一部分：
```swift
for (index, value) in shoppingList.enumerated() {
  print("Item \(index + 1): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

关于`for-in`循环，参见[For-In循环](Control_Flow.md#For-In循环)。

## 无序集合

`set`以无序的方式存放相同类型的不同值。当元素顺序不那么重要或需要确保每个元素只出现一次时，可以用无序集合代替数组。

> 注意：Swift中的`Set`桥接于Foundation框架中的`NSSet`类。
> 更多关于与Foundation和Cocoa一起使用`Set`的信息，参见[桥接Set与NSSet](https://developer.apple.com/documentation/swift/set#2845530)。

### Set类型的哈希值
（关于[哈希表](https://en.wikipedia.org/wiki/Hash_table)和[哈希函数](https://en.wikipedia.org/wiki/Hash_function)）
一个值必须可被哈希，才能被无序集合储存---也就是，该类型必须提供一个计算自身哈希值的方法。哈希值是一个的`Int`值，它对于所有相等的对象来说是相同的，例如，如果`a = b`，则`a.hashValue == b.hashValue`。

Swift所有基本类型（如`String`，`Int`，`Double`和`Bool`）默认都是可哈希的，且都可以用作无序集合的值类型或字典的键类型。枚举的分支值在没有关联值的情况下，默认为可哈希的。

> 注意：
> 可以通过实现Swift标准库中的`Hashable`协议，将自定义类型用作无序集合的类型，或用作字典键类型。实现了`Hashable`协议的类型，需要提供一个名为`hashValue`的可读的`Int`属性。在不同的程序中，或相同程序的不同执行过程中，并不需要`hashValue`属性返回的值都相同。
> 因为`Hashable`协议实现了`Equatable`协议，实现了`Hashable`协议的类型需要提供等号运算符的实现。`Equatable`协议要求等号运算符的任何实现都是等价关系。也就是，对于所有的`a`，`b`和`c`等号运算符的实现需要满足下列三个条件：
> * a == a （自反性）
> * a == b 得出 b == a （对称性）
> * a == b && b == c  得出 a == c （传递性）
>
> 关于实现协议的更多描述，参见[协议](Protocols.md)。

### 无序集合类型语法

Swift中无序集合的类型写作`Set<Element>`，其中`Element`是无序集合允许存放的值的类型。不像数组，无序集合并没有等价的简写格式。

### 创建并初始化一个空无序集合

可以用初始化语法创建一个空的无序集合：
```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// Prints "letters is of type Set<Character> with 0 items."
```

> 注意：
> 从初始化器的类型可以推导出变量`letters`的类型为`Set<Character>`。

另外，如果上下文已经提供了类型信息，如函数参数或已知类型的变量或常量，则可以用空的数组字面量创建空的无序集合。
```swift
letters.insert("a")
// letters now contains 1 value of type Character
letters = []
// letters is now an empty set, but is still of type Set<Character>
```

### 用数组字面量创建无序集合

也可以用数组字面量初始化无序集合，作为书写一个或多个值的无序集合的快捷方法。

下面的例子中，创建一个名为`favoriteGenres`的无序集合，用来存放字符串：
```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hiphop"]
// favoriteGenres has been initialized with three initial items
```

变量`favoriteGenres`被声明为“一个字符串的无序集合”，写作`Set<String>`。因为该无序集合被指定值类型为`String`，所以只允许存放`String`类型的值。这里，无序集合`favoriteGenres`被写作数组字面量的三个字符串值("Rock", "Classical"和"Hip hop")初始化。

> 注意：
> 无序集合`favoriteGenres`被定义成变量(用`var`关键字)而不是常量(用`let`关键字)，是因为后面将有元素被添加或移除。

无序集合的类型不能从单独的数组字面量推导出来，所以无序集合的类型必须显式声明。但是，因为Swift的类型推导，如果你使用只包含单个类型值的数组字面量初始化无序集合，你可以不用写无序集合元素的类型。`favoriteGenres`的初始化可以简写为：
```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

因为数组字面量中所有的值类型相同，Swift可以推导出`Set<String>`是`favoriteGenres`的正确类型。

### 访问和修改无序集合

可以通过方法和属性来访问或修改无序集合。

要知道无序集合元素的数量，用只读属性`count`：
```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// Prints "I have 3 favorite music genres."
```

用布尔属性`isEmpty`作为检查`count`属性是否为`0`的快捷方式：
```swift
if favoriteGenres.isEmpty {
  print("As far as music goes, I'm not picky.")
} else {
  print("I have particular music preferences.")
}
// Prints "I have particular music preferences."
```

用`insert(_:)`方法像无序集合中添加新元素：
```swift
favoriteGenres.insert("Jazz")
// favoriteGenres now contains 4 items
```

通过调用`remove(_:)`方法从无序集合中移除一个元素，如果该元素是无序集合的成员，则移除并返回该值，如果无序集合不包含该元素，则返回`nil`。另外，可以使用`removeAll()`方法移除无序集合中所有元素。
```swift
if let removedGenre = favoriteGenres.remove("Rock") {
  print("\(removedGenre)? I'm over it.")
} else {
  print("I never much cared for that.")
}
// Prints "Rock? I'm over it."
```

要检查无序集合是否包含一个元素，用`contains(_:)`方法：
```swift
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// Prints "It's too funky in here."
```

### 遍历无序集合

可以用`for-in`循环遍历无序集合中所有的值：
```swift
for genre in favoriteGenres {
  print("\(genre)")
}
// Classical
// Jazz
// Hip hop
```
关于`for-in`循环，参见[For-In循环](Control_Flow.md#For-In循环)。

Swift中的无序集合没有固定的顺序。要以指定顺序遍历无序集合，用`sorted()`方法，该方法以`<`操作符排序数组的形式返回无序集合的所有元素。
```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz
```

## 执行无序集合操作

你可以高效地执行基本的无序集合操作，如两个无序集合的结合，取两个无序集合的交集或确定两个无序集合是够包含全部，部分或没有相同元素。

### 无序集合基本操作

下面的插图描绘了两个无序集合---`a`和`b`---图形区域代表无序集合操作的结果。

<p align="center">
<img src="https://docs.swift.org/swift-book/_images/setVennDiagram_2x.png" alt="无序集合基本操作" width="600"/>
</p>

* 用`intersection(_:)`方法创建一个包含两个无序集合共同部分的新无序集合。
* 用`symmetricDifference(_:) `方法创建一个包含在任何一个无序集合中，但不同时在两个无序集合中元素的新无序集合。
* 用`union(_:)`方法创建包含两个无序集合中所有元素的新无序集合。
* 用`subtracting(_:)`方法创建一个不包含某个无序集合中所有元素的新无序集合。
```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

###  无序集合的成员关系和相等

下面的插图描绘了单个无序集合---`a`，`b`和`c`---重合部分表示无序集合共有的元素。集合`a`是集合`b`的超集，因为`a`包含`b`中所有的元素。相反，集合`b`是集合`a`的子集，因为`b`中所有的元素都在`a`中。集合`b`和集合`c`互不相交，因为他们没有共同的元素。

<p align="center">
<img src="https://docs.swift.org/swift-book/_images/setEulerDiagram_2x.png" alt="无序集合基本操作" width="600"/>
</p>

* 用等于运算符(`==`)判断两个集合是否包含全部相同元素。
* 用`isSubset(of:)`方法判断集合中所有元素是否都被包含在指定集合中。
* 用`isSuperset(of:)`方法判断集合是否包含指定集合中的所有元素。
* 用`isStrictSubset(of:)`或`isStrictSuperset(of:)`方法判断一个集合是否是指定集合的子集或超集，但不等于指定集合。
* 用`isDisjoint(with:)`方法判断两个结合是否包含相同元素。

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]

houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```












[< 字符串和字符](Strings_and_Characters.md) || [控制流 >](Control_Flow.md)
