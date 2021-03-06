# JAVASCRIPT FUNCTIONS

 05-20 / 2012

前语：本文是堂主对《Pro JavaScript with Mootools》一书的第二章函数部分知识点讲解的翻译，该书的作者Mark Joseph Obcena是Mootools库的作者和目前开发团队的Leader。个人感觉这部分对Javascript函数的基本知识、内部机制及JavaScript解析器的运行机制讲的比较明白，脉络也清楚，对初学者掌握JavaScript函数基础知识很有好处。尤其难得的是不同于其他JavaScript书籍讲述的都是分散的知识点，这本书的知识讲解是有清晰、循序渐进的脉络的。换句话说，这本书中的JavaScript知识是串起来的。

虽然这本出版了近2年的书籍国内并未正式引进，但我依然建议有需求的可以从amazon上自行买来看一下，或者网上搜一下PDF的版本（确实有PDF全版下载的）。我个人则是花了近300大洋从amazon上买了一本英文原版的，没办法，更喜欢纸质版的书籍。这本书其实可以理解为“基于MooTools实践项目的JavaScript指南”，总的脉络是“JavaScript基础知识 - 高级技巧 - MooTools对原生JavaScript的改进”，非常值得一读。

本篇译文字数较多，大概2万字，我不知道能有几位看官有耐心看完。如果真有，且发现堂主一些地方翻译的不对或有优化建议，欢迎留言指教，共同成长。另外，非本土产技术类书籍，还是建议直接读英文原版，翻译的再好也比不了原版的意境。

下面是译文正式内容：

---------------------------------------------------------------------

JavaScript最好的特色之一是其对函数的实现。不同于其他编程语言为不同情景提供不同的函数类型，JavaScript只为我们提供了一种涵盖所有情景（如内嵌函数、匿名函数或是对象方法）的函数类型。 请不要被一个从外表看似简单的JavaScript函数迷惑——这个基本的函数声明如同一座城堡，隐藏着非常复杂的内部操作。我们整章都是围绕着诸如函数结构、作用域、执行上下文以及函数执行等这些在实际操作中需要重点去考虑的问题来讲述的。搞明白这些平时会被忽略的细节不但有助你更加了解这门语言，同时也会为你在解决异常复杂的问题时提供巨大帮助。

## 关于函数（The Function）

最开始，我们需要统一一些基本术语。从现在开始，我们将**函数(functions)**的概念定义为“执行一个明确的动作并提供一个返回值的独立代码块”。函数可以接收作为值传递给它的**参数(arguments)**，函数可以被用来提供**返回值(return value)**，也可以通过**调用(invoking)**被多次执行。

    // 一个带有2个参数的基本函数:
    function add(one, two) {
        return one + two;
    }
    
    // 调用这个函数并给它2个参数:
    var result = add(1, 42);
    console.log(result); // 43
    
    // 再次调用这个函数，给它另外2个参数
    result = add(5, 20);
    console.log(result); // 25

JavaScript是一个将函数作为**一等对象(first-class functions)**的语言。一个一等对象的函数意味着函数可以储存在变量中，可以被作为参数传递给其他函数使用，也可以作为其他函数的返回值。这么做的合理性是因为在JavaScript中随处可见的函数其实都是对象。这门语言还允许你创建新的函数并在运行时改变函数的定义。

## 一种函数，多种形式（One Function, Multiple Forms）

虽然在JavaScript中只存在一种函数类型，但却存在多种函数形式，这意味着可以通过不同的方式去创建一个函数。这些形式中最常见的是下面这种被称为**函数字面量(function literal)**的创建语法：

    function Identifier(FormalParamters, ...) {
        FunctionBody
    }

首先是一个function关键字后面跟着一个空格，之后是一个自选的**标识符(identifier)**用以说明你的函数；之后跟着的是以逗号分割的**形参(formal parameters)**列表，该形参列表处于一对圆括号中，这些形参会在函数内部转变为可用的局部变量；最后是一个自选的**函数体(funciton body)**，在这里面你可以书写声明和表达式。请注意下面的说法是正确的：一个函数有多个可选部分。我们现在还没针对这个问题进行详细的说明，因为对其的解答将贯穿本章。

> 注意：在本书的很多章节我们都会看到**字面量(literal)**这个术语。在JavaScript中，字面量是指在你代码中明确定义的值。“mark”、1或者true是字符串、数字和布尔字面量的例子，而function()和[1, 2]则分别是函数和数组字面量的例子。

在标识符（或后面我们会见到的针对这个对象本身）后面使用调用**操作符(invocation operator)** “()”的被称为一个函数。同时调用操作符()也可以为函数传递**实参(actual arguments)**。

> 注意：一个函数的形参是指在创建函数时圆括号中被声明的有命名的变量，而实参则是指函数被调用时传给它的值。

因为函数同时也是对象，所以它也具有方法和属性。我们将在本书第三章更多的讨论函数的方法和属性，这里只需要先记住函数具有两个基本的属性：

1. 名称(name)：保存着函数标识符这个字符串的值
2. 长度(length)：这是一个关于函数形参数量的整数（如果函数没有形参，其length为0）
函数声明（Function Declaration）

采用基本语法，我们创建第一种函数形式，称之为**函数声明(function declaration)**。函数声明是所有函数形式中最简单的一种，且绝大部分的开发者都在他们的代码中使用这种形式。下面的代码定义了一个新的函数，它的名字是“add”：

    // 一个名为“add”的函数
    function add(a, b) {
        return a + b;
    }
    
    console.log(typeof add); // 'function'
    console.log(add.name); // 'add'
    console.log(add.length); // '2'
    console.log(add(20, 5)); // '25'

在函数声明中需要赋予被声明的函数一个标识符，这个标识符将在当前作用域中创建一个值为函数的变量。在我们的例子中，我们在全局作用域中创建了一个add的变量，这个变量的name属性值为add，这等价于这个函数的标识符，且这个函数的length为2，因为我们为其设置了2个形参。 因为JavaScript是基于词法作用域(lexically scoped)的，所以标识符被固定在它们被定义的作用域而不是语法上或是其被调用时的作用域。记住这一点很重要，因为JavaScript允许我们在函数中定义函数，这种情况下关于作用域的规则可能会变得不易理解。

    // 外层函数，全局作用域
    function outer() {
    
        // 内层函数，局部作用域
        function inner() {
            // ...
        }
    
    }
    
    // 检测外层函数
    console.log(typeof outer); // 'function'
    
    // 运行外层函数来创建一个新的函数
    outer();
    
    // 检测内层函数
    console.log(typeof inner); // 'undefined'

在这个例子中，我们在全局作用域中创建了一个outer变量并为之赋值为outer函数。当我们调用它时，它创建了一个名为inner的局部变量，这个局部变量被赋值为inner函数，当我们使用typeof操作符进行检测的时候，在全局作用域中outer函数是可以被有效访问的，但inner函数却只能在outer函数内部被访问到——这是因为inner函数只存在于一个局部作用域中。 因为函数声明同时还创建了一个同名的变量作为他的标识符，所以你必须确定在当前作用域不存在其他同名标识符的变量。否则，后面同名变量的值会覆盖前面的：

    // 当前作用域中的一个变量
    var items = 1;
    
    // 一个被声明为同名的函数
    function items() {
        // ...
    };
    
    console.log(typeof items); // 'function' 而非 'number'

我们过一会会讨论更多关于函数作用域的细节，现在我们看一下另外一种形式的函数。

## 函数表达式(Function Expression)

下面要说的函数形式具备一定的优势，这个优势在于函数被储存在一个变量中，这种形式的函数被称为函数表达式(funciton expression)。不同于明确的声明一个函数，这时的函数以一个变量返回值的面貌出现。下面是一个和上面一样的add函数，但这次我们使用了函数表达式：

    var add = function(a, b) {
        return a + b;
    };
    
    console.log(typeof add); // 'function'
    console.log(add.name); // '' 或 'anonymous'
    console.log(add.length); // '2'
    console.log(add(20, 5)); // '25'

这里我们创建了一个函数字面量作为add这个变量的值，下面我们就可以使用这个变量来调用这个函数，如最后的那个语句展示的我们用它来求两个数的和。 你会注意到它的length属性和对应的函数声明的length属性是一样，但是name属性却不一样。在一些JavaScript解析器中，这个值会是空字符串，而在另一些中则会是“anonymous”。发生这种情况的原因是我们并未给一个函数字面量指定一个标识符。在JavaSrcipt中，一个未使用明确标识符的函数被称为一个匿名函数(anonymous)。 函数表达式的作用域规则不同于函数声明的作用域规则，这是因为其取决于被赋值的那个变量的作用域。记住在JavaScript中，由关键字var声明的变量是一个局部变量，而忽略了这个关键字则会创建一个全局变量。

    // 外层函数，全局作用域
    var outer = function() {

        // 内层函数，局部作用域
        var localInner = function() {
            // ...
        }; 
    
        // 内层函数，全局作用域
        globalInner = function() {
            // ...
        };

    }

    // 检测外层函数
    console.log(typeof outer); // 'function'
    
    // 运行外层函数来创建一个新的函数
    outer();
    
    // 检测新的函数
    console.log(typeof localInner); // 'undefined'
    console.log(typeof globalInner); // 'function'

outer函数被定义在全局作用域中，这是因为虽然我们使用了var关键字，但其在当前应用中处于最高层级。在这个函数内部有另外的2个函数：localInner和globalInner。localInner函数被赋值给一个局部变量，在outer外部无法访问它。而globalIner则因在定义时缺失var关键字，其结果是这个变量及其引用的函数都处于全局作用域中。

## 命名的函数表达式(Named Function Expression)

虽然函数表达式经常被书写为采用匿名函数的形式，但你依然可以为这个匿名函数赋予一个明确的标识符。这个函数表达式的变种被称为一个**命名的函数表达式(named function expression)**。

    var add = function add(a, b) {
        return a + b;
    };
    
    console.log(typeof add); // 'function'
    console.log(add.name); // 'add'
    console.log(add.length); // '2'
    console.log(add(20, 5)); //'25'

这个例子和采用匿名函数方式的函数表达式是一样的，但我们为函数字面量赋予了一个明确的标识符。和前一个例子不同，这时的name属性的值是“add”，这个值同我们为其赋予的那个标识符是一致的。 JavaScript允许我们为匿名函数赋予一个明确的标识符，这样就可以在这个函数内部引用其本身。你可能会问为什么我们需要这个特征，下面让我们来看两个例子：

    var myFn = function() {
        // 引用这个函数
        console.log(typeof myFn);
    };
    myFn(); // 'function'

上面的这个例子，myFn这个函数可以轻松的通过它的变量名来引用，这是因为它的变量名在其作用域中是有效的。不过，看一下下面的这个例子：

    // 全局作用域
    var createFn = function() {
    
        // 返回函数
        return function() {
            console.log(typeof myFn);
        };
    
    };
    
    // 不同的作用域
    (function() {
    
        // 将createFn的返回值赋予一个局部变量
        var myFn = createFn();
    
        // 检测引用是否可行
        myFn(); // 'undefined'
    
    })();

这个例子可能有点复杂，我们稍后会讨论它的细节。现在，我们只关心函数本身。在全局作用域中，我们创建了一个createFn函数，它返回一个和前面例子一样的log函数。之后我们创建了一个匿名的局部作用域，在其中定义了一个变量myFn，并把createFn的返回值赋予这个变量。 这段代码和前面那个看起来很像，但不同的是我们没使用一个被明确赋值为函数字面量的变量，而是使用了一个由其他函数产生的返回值。而且，变量myFn是在一个不同的局部作用域中，在这个作用域中访问不到上面createFn函数作用域中的返回值。因此，在这个例子中，log函数不会返回“function”而是会返回一个“undefined”。 通过为匿名函数设置一个明确的标识符，即使我们通过持有它的变量访问到它，也可以去引用这个函数自身。

    // 全局作用域
    var createFn = function() {
    
        // 返回函数
        return function myFn() {
            console.log(typeof myFn);
        };
    
    };
    
    // 不同的作用域
    (function() {
    
        // 将createFn的返回值赋予一个局部变量
        var myFn = createFn();
    
        // 检测引用是否可行
        myFn(); // 'function'
    
    })();

添加一个明确的标识符类似于创建一个新的可访问该函数内部的变量，使用这个变量就可以引用这个函数自身。这样使得函数在其内部调用自身（用于递归操作）或在其本身上执行操作成为可能。 一个命名了的函数声明同一个采用匿名函数形式的函数声明具有相同的作用域规则：引用它的变量作用域决定了这个函数是局部的或是全局的。

    // 一个有着不同标识符的函数
    var myFn = function fnID() {
        console.log(typeof fnID);
    };
    
    // 对于变量
    console.log(typeof myFn); // 'function'
    
    // 对于标识符
    console.log(typeof fnID); // 'undefined'
    
    myFn(); // 'function'

这个例子显示了，通过变量myFn可以成功的引用函数，但通过标识符fnID却无法从外部访问到它。但是，通过标识符却可以在函数内部引用其自身。

## 自执行函数(Single-Execution Function)

我们在前面介绍函数表达式时曾接触过匿名函数，其还有着更广泛的用处。其中最重要的一项技术就是使用匿名函数创建一个立即执行的函数——且不需要事先把它们先存在变量里。这种函数形式我们称之为自执行函数(single-execution function)。

    // 创建一个函数并立即调用其自身
    (function() {
    
        var msg = 'Hello World';
        console.log(msg);
    
    })();

这里我们创建了一个函数字面量并把它包裹在一对圆括号中。之后我们使用函数调用操作符()来立即执行这个函数。这个函数并未储存在一个变量里，或是任何针对它而创建的引用。这是个“一次性运行”的函数：创造它，执行它，之后继续其他的操作。

要想理解自执行函数是如何工作的，你要记住函数都是对象，而对象都是值。因为在JavaScript中值可以被立即使用而无需先被储存在变量里，所以你可以在一对圆括号中插入一个匿名函数来立即运行它。

但是，如果我们像下面这么做：

    // 这么写会被认为是一个语法错误
    function() {
    
        var msg = 'Hello World';
        console.log(msg);
    
    }();

当JavaScript解析器遇到这行代码会抛出一个语法错误，因为解析器会把这个函数当成一个函数声明。这看起来是一个没有标识符的函数声明，而因为函数声明的方式必须要在function关键字之后跟着一个标识符，所以解析器会抛出错误。

我们把函数放在一对圆括号中来告诉解析器这不是一个函数声明，更准确的说，我们创建了一个函数并立即运行了它的值。因为我们没有一个可用于调用这个函数的标识符，所以我们需要把函数放在一对圆括号中以便可以创建一个正确的方法来调用到这个函数。这种包围在外层的圆括号应该出现在我们没有一个明确的方式来调用函数的时候，比如我们现在说的这种自执行函数。

> 注意：执行操作符()可以既可以放在圆括号外面，也可以放在圆括号里面，如：(function() {…}())。但一般情况下大家更习惯于把执行操作符放在外面。

自执行函数的用处很多，其中最重要的一点是为变量和标识符创造一个受保护的局部作用域，看下面的例子：

    // 顶层作用域
    var a = 1;
    
    // 一个由自执行函数创建的局部作用域
    (function() {
    
        //局部作用域
        var a = 2;
    
    })();
    
    console.log(a); // 1

这里，外面先在顶层作用域创建了一个值为1的变量a，之后创建一个自执行函数并在里面再次声明一个a变量并赋值为2。因为这是一个局部作用域，所以外面的顶层作用域中的变量a的值并不会被改变。

这项技术目前很流行，尤其对于JavaScript库(library)的开发者，因为局部变量进入一个不同作用域时需要避免标识符冲突。

另一种自执行函数的用处是通过一次性的执行来为你提供它的返回值：

    // 把一个自执行函数的返回值保存在一个变量里
    var name = (function(name) {
    
        return ['Hello', name].join(' ');
    
    })('Mark');
    
    console.log(name); // 'Hello Mark'

别被这段代码迷惑到：我们这里不是创建了一个函数表达式，而是创建了一个自执行函数并立即执行它，把它的返回值赋予变量name。

自执行函数另一个特色是可以为它配置标识符，类似一个函数声明的做法：

    (function myFn() {
    
        console.log(typeof myFn); // 'function'
    
    })();
    
    console.log(myFn); // 'undefined'

虽然这看起来像是一个函数声明，但这却是一个自执行函数。虽然我们为它设置了一个标识符，但它并不会像函数声明那样在当前作用域创建一个变量。这个标识符使得你可以在函数内部引用其自身，而不必另外在当前作用域再新建一个变量。这对于避免覆盖当前作用域中已存在的变量尤其有好处。

同其他的函数形式一样，自执行函数也可以通过执行操作符来传递参数。通过在函数内部把函数的标志符作为一个变量并把该函数的返回值储存在该变量中，我们可以创建一个递归的函数。

    var number = 12;
    
    var numberFactorial = (function factorial(number) {
         return (number === 0) ? 1 : number * factorial(number - 1);
    })(number);
    
    console.log(numberFactorial); //479001600

## 函数对象(Function Object)

最后一种函数形式，就是**函数对象(funciton object)**，它不同于上面几种采用函数字面量的方式，这种函数形式的语法如下：

    // 一个函数对象
    new Function('FormalArgument1', 'FormalArgument2',..., 'FunctionBody');

这里，我们使用Function的构造函数创建了一个新的函数并把字符串作为参数传递给它。前面的已经命名的参数为新建函数对象的参数，最后一个参数为这个函数的函数体。

> 注意：虽然这里我们把这种形式成为函数对象，但请记住其实所有的函数都是对象。我们在这里采用这个术语的目的是为了和函数字面量的方式进行区分。

下面我们采用这种形式创建一个函数：

    var add = new Function('a', 'b', 'return a + b;');
    
    console.log(typeof add); // 'function'
    console.log(add.name); // '' 或 'anonymous'
    console.log(add.length); // '2'
    console.log(add(20, 5)); // '25'

你可能会发现这种方式比采用函数字面量方式创建一个匿名函数要更简单。和匿名函数一样，对其检测name属性会得到一个空的字符串或anonymous。在第一行，我们使用Function的构造函数创建了一个新的函数，并赋值给变量add。这个函数接收2个参数a和b，会在运行时将a和b相加并把相加结果做作为函数返回值。

使用这种函数形式类似于使用eval：最后的一个字符串参数会在函数运行时作为函数体里的代码被执行。

注意：你不是必须将命名的参数作为分开的字符串传递，Function构造函数也允许一个字符串里包含多个以逗号分隔的项这种传参方式。比如：new Function(‘a, b’, ‘return a + b;’);

虽然这种函数形式有它的用处，但其相比函数字面量的方式存在一个显著的劣势，就是它是处在全局作用域中的：

    // 全局变量
    var x = 1;
    
    // 局部作用域
    (function() {
    
        // 局部变量
        var x = 5;
        var myFn = new Function('console.log(x)');
        myFn(); // 1, not 5
    
    })();

虽然我们在独立的作用域中定义了一个局部变量，但输出结果却是1而非5，这是因为Function构造函数是运行在全局作用域中。

## 参数（Arguments）

所有函数都能从内部访问到它们的实参。这些实参会在函数内部变为一个个局部变量，其值是函数在调用时传进来的那个值。另外，如果函数在调用时实际使用的参数少于它在定义时确定的形参，那么那些多余的未用到的参数的值就会是undefined。

    var myFn = function(frist, second) {
        console.log('frist : ' + frist);
        console.log('second : ' + second);
    };
    
    myFn(1, 2);
    // first : 1
    // second : 2
    
    myFn('a', 'b', 'c');
    // first : a
    // second : b
    
    myFn('test');
    // first : test
    // second : undefined

因为JavaScript允许向函数传递任意个数的参数，这也同时为我们提供了一个方式来判断函数在调用时使用的实参和函数定义时的形参的数量是否相同。这个检测的方式通过arguments这个对象来实现，这个对象类似于数组，储存着该函数的实参：

    var myFn = function(frist, second) {
        console.log('length : ' + arguments.length);
        console.log('frist : ' + arguments[0]);
    };
    
    myFn(1, 2);
    // length : 2
    // frist : 1
    
    myFn('a', 'b', 'c');
    // length : 3
    // frist : a
    
    myFn('test');
    // length : 2
    // frist : test

arguments对象的length属性可以显示我们传递函数的实参个数。对实参的调用可以对arguments对象使用类似数组的下标法：arguments[0]表示传递的第一个实参，arguments[1]表示第二个实参。

使用arguments对象取代有名字的参数，你可以创建一个可以对不同数量参数进行处理的函数。比如可以使用这种技巧来帮助我们改进前面的那个add函数，使得其可以对任意数量的参数进行累加，最后返回累加的值：

    var add = function(){
        var result = 0,
            len = arguments.length;
    
        while(len--) result += arguments[len];
        console.log(result);
    }; 
    
    add(15); // 15
    add(31, 32, 92); // 135
    add(19, 53, 27, 41, 101); // 241

arguments对象有一个很大的问题需要引起你的注意：它是一个可变的对象，你可以改变其内部的参数值甚至是把它整个变成另一个对象：

    var rewriteArgs = function() {
        arguments[0] = 'no';
        console.log(arguments[0]);
    };
    
    rewriteArgs('yes'); // 'no'
    
    var replaceArgs = function() {
        arguments = null;
        console.log(arguments === null);
    };
    
    replaceArgs(); // 'true'

上面第一个函数向我们展示了如果重置一个参数的值；后面的函数向我们展示了如何整体更改一个arguments对象。对于arguments对象来说，唯一的固定属性就是length了，即使你在函数内部动态的增加了arguments对象里的参数，length依然只显示函数调用时赋予的实参的数量。

    var appendArgs = function() {
        arguments[2] = 'three';
        console.log(arguments.length);
    };
    
    appendArgs('one', 'two'); // 2

当你写代码的时候，请确保没有更改arguments内的参数值或覆盖这个对象。

对于arguments对象还有另一个属性值：callee，这是一个针对该函数自身的引用。在前面的代码中我们使用函数的标识符来实现在函数内部引用其自身，现在我们换一种方式，使用arguments.callee：

    var number = 12;
    
    var numberFactorial = (function(number) {
        return (number === 0) ? 1 : number * arguments.callee(number - 1);
    })(number);
    
    console.log(numberFactorial); //479001600

注意这里我们创建的是一个匿名函数，虽然我们没有函数标识符，但依然可以通过arguments.callee来准确的引用其自身。创建这个属性的意图就是为了能在没有标识符可供使用的时候（或者就算是有一个标识符时也可以使用callee）来提供一个有效方式在函数内部引用其自身。

虽然这是一个很有用的属性，但在新的ECMAScript 5的规范中，arguments.callee属性却被废弃了。如果使用ES5的严格模式，该属性会引起一个报错。所以，除非真的是有必要，否则轻易不要使用这个属性，而是用我们前面说过的方法使用标识符来达到同样的目的。

虽然JavaScript允许给函数传递很多参数，但却并未提供一个设置参数默认值的方法，不过我们可以通过判断参数值是否是undefined来模拟配置默认值的操作：

    var greet = function(name, greeting) {

        // 检测参数是否是定义了的
        // 如果不是，就提供一个默认值
        name = name || 'Mark';
        greeting = greeting || 'Hello';
    
        console.log([greeting, name]).join(' ');

    };

    greet('Tim', 'Hi'); // 'Hi Tim'
    greet('Tim');       // 'Hello Tim'
    greet();            // 'Hello Mark'
    
因为未在函数调用时赋值的参数其值为undefined，而undefined在布尔判断时返回的是false，所以我们可以使用逻辑或运算符 || 来为参数设置一个默认值。

另外一点需要特别注意的是，原生类型的参数（如字符串和整数）是以值的方式来传递的，这意味着这些值的改变不会对外层作用域引起反射。不过，作为参数使用的函数和对象，则是以他们的引用来传递，在函数作用域中的对参数的任何改动都会引起外层的反射：

    var obj = {name : 'Mark'};
    
    var changeNative = function(name) {
        name = 'Joseph';
        console.log(name);
    };
    
    changeNative(obj.name); // 'Joseph'
    
    console.log(obj.name); // 'Mark'
    
    var changeObj = function(obj) {
        obj.name = 'joseph';
        console.log(obj.name);
    };
    
    changeObj(obj); // 'Joseph'
    
    console.log(obj.name); // 'Joseph'

第一步我们将obj.name作为参数传给函数，因为其为一个原生的字符串类型，其传递的是它值的拷贝（储存在栈上），所以在函数内部对其进行改变不会对外层作用域中的obj产生影响。而接下来我们把obj对象本身作为一个参数传递，因为函数和对象等在作为参数进行传递时其传递的是对自身的引用（储存在堆上），所以局部作用域中对其属性值的任何更改都会立即反射到外层作用域中的obj对象。

最后，你可能会说之前我曾提到过arguments对象是类数组的。这意味着虽然arguments对象看起来像数组（可以通过下标来用于），但它没有数组的那些方法。如果你喜欢，你可以用数组的Array.prototype.slice方法把arguments对象转变为一个真正的数组：

    var argsToArray = function() {
        console.log(typeof arguments.callee); // 'function'
        var args = Array.prototype.slice.call(arguments);
        console.log(typeof arguments.callee); // 'undefined'
        console.log(typeof arguments.slice); // 'function'
    };
    
    argsToArray();

## 返回值（Return Values）

Return关键字用来为函数提供一个明确的返回值，JavaScript允许在函数内部书写多个return关键字，函数会再其中一个执行后立即退出。

    var isOne = function(number) {
       if (number === 1) return true;
    
       console.log('Not one ..');
       return false;
    };
    
    var one = isOne(1);
    console.log(one); // true
    
    var two = isOne(2); // Not one ..
    console.log(two); // false

在这个函数第一次被引用时，我们传进去一个参数1，因为我们在函数内部先做了一个条件判断，当前传入的参数1使得该条件判断语句返回true，于是 return true代码会被执行，函数同时立即停止。在第二次引用时我们传进去的参数2不符合前面的条件判断语句要求，于是函数会一直执行到最后的return false代码。

在函数内部设置多个return语句对于函数分层执行是很有好处的。这同时也被普遍应用于在函数运行最开始对必须的变量进行检测，如有不符合的情况则立即退出函数执行，这既能节省时间又能为我们提供一个错误提示。下面的这个例子就是一段从DOM元素中获取其自定义属性值的代码片段：

    var getData = function(id) {
        if (!id) return null;
        var element = $(id);
        if (!element) return null;
        return element.get('data-name');
    };
    
    console.log(getData()); // null
    console.log(getData('non existent id')); // null
    console.log(getData('main')); // 'Tim'

组后关于函数返回值要提醒各位的一点是：不论你希望与否，函数总是会提供一个返回值。如果未显示地设置return关键字或设置的return未有机会执行，则函数会返回一个undefined。

## 函数内部（Function Internals）

我们前面讨论过了函数形式、参数以及函数的返回值等与函数有关的核心话题，下面我们要讨论一些代码之下的东西。在下面的章节里，我们会讨论一些函数内部的幕后事务，让我们一起来偷窥下当JavaScript解析器进入一个函数时会做些什么。我们不会陷入针对细节的讨论，而是关注那些有利于我们更好的理解函数概念的那些重要的点。

有些人可能会觉得在最开始接触JavaScript的时候，这门语言在某些时候会显得不那么严谨，而且它的规则也不那么好理解。了解一些内部机制有助于我们更好的理解那些看起来随意的规则，同时在后面的章节里会看到，了解JavaScript的内部工作机制会对你书写出可靠的、健壮的代码有着巨大的帮助。

> 注意：JavaScript解析器在现实中的工作方式会因其制造厂商不同而不相一致，所以我们下面要讨论的一些解析器的细节可能不全是准确的。不过ECMAScript规范对解析器应该如何执行函数提供了基本的规则描述，所以对于函数内部发生的事，我们是有着一套官方指南的。

## 可执行代码和执行上下文(Executable Code and Execution Contexts)

JavaScript区分三种可执行代码：

- 全局代码(Global code)是指出现在应用代码中顶层的代码。
- 函数代码(Function code)是指在函数内部的代码或是在函数体之前被调用的代码。
- Eval代码(Eval code)是指被传进eval方法中并被其执行的代码。

下面的例子展示了这三种不同的可执行代码：

    // 这是全局代码
    var name = 'John';
    var age = 20;
    
    function add(a, b) {
        // 这是函数代码
        var result = a + b;
        return result;
    }
    
    (function() {
        // 这是函数代码
        var day = 'Tuesday';
        var time = function() {
            // 这还是函数代码
            // 不过和上面的代码在作用域上是分开的
            return day;
        };
    })();
    
    // 这是eval代码
    eval('alert("yay!");');

上面我们创建的name、age以及大部分的函数都在顶层代码中，这意味着它们是全局代码。不过，处于函数中的代码是函数代码，它被视为同全局代码是相分隔的。函数中内嵌的函数，其内部代码同外部的函数代码也被视为是相分隔的。

那么为什么我们需要对JavaScript中的代码进行分类呢？这是为了在解析器解析代码时能够追踪到其当前所处的位置，JavaScript解析器采用了一个被称为**执行上下文(execution context)**的内部机制。在处理一段脚本的过程中，JavaScript会创建并进入不同的执行上下文，这个行为本身不仅保存着它运行到这个函数当前位置所经过的轨迹，同时还储存着函数正常运行所需要的数据。

每个JavaScript程序都至少有一个执行上下文，通常我们称之为**全局执行上下文(global execution context)**，当一个JavaScript解析器开始解析你的程序的时候，它首先“进入”全局执行上下文并在这个执行上下文环境中处理代码。当它遇到一个函数，它会创建一个新的执行上下文并进入这个上下文利用这个环境来执行函数代码。当函数执行完毕或者遇到一个return结束之后，解析器会退出当先的执行上下文并回到之前所处的那个执行上下文环境。

这个看起来不是很好理解，我们下面用一个简单的例子来把它理清：

    var a = 1;
    
    var add = function(a, b) {
        return a + b;
    };
    
    var callAdd = function(a, b) {
        return add(a, b);
    };
    
    add(a, 2);
    
    call(1, 2);

这段简单的代码不单足够帮助我们来理解上面说的事情，同时还是一个很好的例子来展示JavaScript是如何创建、进入并离开一个执行上下文的。让我们一步一步来分析：

当程序开始执行，Javascript解析器首先进入全局执行上下文并在这里解析代码。它会先创建三个变量a、add、callAdd，并分别为它们赋值为数字1、一个函数和另一个函数。
解析器遇到了一个针对add函数的调用。于是解析器创建了一个新的执行上下文，进入这个上下文，计算a + b表达式的值，之后返回这个表达式的值。当这个值被返回后，解析器离开了这个它新创建的执行上下文，把它销毁掉，重新回到全局执行上下文。
接下来解析器遇到了另一个函数调用，这次是对callAdd的调用。像第二步一样，解析器会新创建一个执行上下文，并在它解析callAdd函数体中的代码之前先进入这个执行上下文。当它对函数体内的代码进行处理的时候，遇到了一个新的函数调用——这次是对add的调用，于是解析器会再新建一个执行上下文并进入这里。此时，我们已有了三个执行上下文：一个全局执行上下文、一个针对callAdd的执行上下文，一个针对add函数的执行上下文。最后一个是当前被激活的执行上下文。当add函数调用执行完毕后，当前的执行上下文会被销毁并回到callAdd的执行上下文中，callAdd的执行上下文中的运行结果也是返回一个值，这通知解析器退出并销毁当前的执行上下文，重新回到全局执行上下文中。
执行上下文的概念对于一个在代码中不会直接面对它的前端新人来说，可能是会有一点复杂，这是可以理解的。你此时可能会问，那既然我们在编程中不会直接面对执行上下文，那我们又为什么要讨论它呢？

答案就在于执行上下文的其他那些用途。我在前面提到过JavaScript解析器依靠执行上下文来保存它运行到当前位置所经过的轨迹，此外一些程序内部相互关联的对象也要依靠执行上下文来正确处理你的程序。

## 变量和变量初始化(Variables and Variable Instantition)

这些内部的对象之一就是**变量对象(variable object)**。每一个执行上下文都拥有它自己的变量对象用来记录在当前上下文环境中定义的变量。

在JavaScript中创建变量的过程被称为变量初始化(variable instantition)。因为JavaScript是基于词法作用域的，这意味着一个变量所处的作用域由其在代码中被实例化的位置所决定。唯一的例外是不采用关键字var创建的变量是全局变量。

    var fruit = 'banana';
    
    var add = function(a, b) {
        var localResult = a + b;
        globalResult = localResult;
        return localResult;
    };
    
    add(1, 2);

在这个代码片段中，变量fruit和函数add处于全局作用域中，在整个脚本中都能被访问到。而对于变量localResult、a、b则是局部变量，只能在函数内部被访问到。而变量globalResult因为在声明时缺少关键字var，所以它会成为一个全局变量。

当JavaScript解析器进入一个执行上下文中，首先要做的就是变量初始化操作。解析器首先会在当前的执行上下文中创建一个variable对象，之后在当前上下文环境中搜索var声明，创建这些变量并添加进之前创建的variable对象中，此时这些变量的值都被设置为undefined。让我们审视一下我们的演示代码，我们可以说变量fruit和add通过variable对象在当前执行上下文中被初始化，而变量localResult、a、b则通过variable对象在add函数的上下文空间中被初始化。而globalResult则是一个需要被特别注意的变量，这个我们一会再来讨论它。

关于变量初始化有很重要的一点需要我们去记住，就是它同执行上下文是紧密结合的。回忆一下，前面我们对JavaScript划分了三种不同的执行代码：全局代码、函数代码和eval代码。同理，我们也可以说存在着三种不同的执行上下文：全局执行上下文、函数执行上下文、eval执行上下文。因为变量初始化是通过处于执行上下文中的variable对象实现的，进而可以说也存在着三种类型的变量：全局变量、处于函数作用域中的变量以及来自eval代码中的变量。

这为我们引出了很多人对这门语言感觉困惑的那些问题中一个：JavaScript没有块级作用域。在其他的类C语言中，一对花括号中的代码被称为一个**块(block)**，块有着自己独立的作用域。因为变量初始化发生在执行上下文这一层级中，所以在当前执行上下文中任意位置被初始化的变量，在这整个上下文空间中（包括其内部的其他子上下文空间）都是可见的：

    var x = 1;
    
    if (false) {
        var y =2;
    }
    
    console.log(x); // 1
    console.log(y); // undefined

在拥有块级作用域的语言中，console.log(y)会抛出一个错误，因为条件判断语句中的代码是不会被执行的，那么变量y自然也不会被初始化。但在JavaScript中这并不会抛出一个错误，而是告诉我们y的值是undefined，这个值是一个变量已经被初始化但还未被赋值时所具有的默认值。这个行为看起来挺有意思，不是么？

不过，如果我们还记得变量初始化是发生在执行上下文这一层级中，我们就会明白这种行为其实正是我们所期望的。当JavaScript开始解析上面的代码块的时候，它首先会进入全局执行上下文，之后在整个上下文环境中寻找变量声明并初始化它们，之后把他们加入variable对象中去。所以我们的代码实际上是像下面这样被解析的：

    var x;
    var y;
    
    x = 1;
    
    if (false) {
        y = 2;
    }
    
    console.log(x); // 1
    console.log(y); // undefined

同样的在上下文环境中的初始化也适用于函数：
    
    function test() {
        console.log(value); // undefined
        var value = 1;
        console.log(value); // 1
    }
    
    test();

虽然我们对变量的赋值操作是在第一行log语句之后才进行的，但第一行的log还是会给我们返回一个undefined而非一个报错。这是因为变量初始化是先于函数内其他任何执行代码之前进行的。我们的变量会在第一时间被初始化并被暂时设置为undefined，其到了第二行代码被执行时才被正式赋值为1。所以说将变量初始化的操作放在代码或函数的最前面是一个好习惯，这样可以保证在当前作用域的任何位置，变量都是可用的。

就像你见到的，创建变量的过程（初始化）和给变量赋值的过程（声明）是被JavaScript解析器分开执行的。我们回到上一个例子：

    var add = function(a, b) {
        var localResult = a + b;
        globalResult = localResult;
        return localResult;
    };
    
    add(1, 2);

在这个代码片段中，变量localResult是函数的一个局部变量，但是globalResult却是一个全局变量。对于这个现象最常见的解释是因为在创建变量时缺少关键字var于是变量成了全局的，但这并不是一个靠谱的解释。现在我们已经知道了变量的初始化和声明是分开进行的，所以我们可以从一个解析器的视角把上面的代码重写：

    var add = function(a, b) {
        var localResult;
        localResult = a + b;
        globalResult = localResult;
        return localResult;
    };
    
    add(1, 2);

变量localResult会被初始化并会在当前执行上下文的variable对象中创建一个针对它的引用。当解析器看到“localResult = a + b;”这一行时，它会在当前执行上下文环境的variable对象中检查是否存在一个localResult对象，因为现在存在这么一个变量，于是这个值（a + b）被赋给了它。然而，当解析器遇到“globalResult = localResult;”这一行代码时，它不论在当前环境的variable对象中还是在更上一级的执行上下文环境（对本例来说是全局执行上下文）的variable对象中都没找到一个名为globalResult的对象引用。因为解析器始终找不到这么一个引用，于是它认为这是一个新的变量，并会在它所寻找的最后一层执行上下文环境——总会是全局执行上下文——中创建这么一个新的变量。于是，globalResult最后成了一个全局变量。

## 作用域和作用域链(Scoping and Scope Chain)

在执行上下文的作用域中查找变量的过程被称为**标识符解析(indentifier resolution)**，这个过程的实现依赖于函数内部另一个同执行上下文相关联的对象——**作用域链(scope chain)**。就像它的名字所蕴含的那样，作用域链是一个有序链表，其包含着用以告诉JavaScript解析器一个标识符到底关联着哪一个变量的对象。

每一个执行上下文都有其自己的作用域链，该作用域链在解析器进入该执行上下文之前就已经被创建好了。一个作用域链可以包含数个对象，其中的一个便是当前执行上下文的variable对象。我们看一下下面的简单代码：

    var fruit = 'banana';
    var animal = 'cat';
    
    console.log(fruit); // 'banana'
    console.log(animal); // 'cat'

这段代码运行在全局执行上下文中，所以变量fruit和animal储存在全局执行上下文的variable对象中。当解析器遇到“console.log(fruit);”这段代码，它看到了标识符fruit并在当前的作用域链（目前只包含了一个对象，就是当前全局执行上下文的variable对象）中寻找这个标识符的值，于是接下来解析器发现这个变量有一个内容为“banana”的值。下一行的log语句的执行过程同这个是一样的。

同时，全局执行上下文中的variable对象还有另外一个用途，就是被用做global对象。解析器对global对象有其自身的内部实现方式，但依然可以通过JavaScript在当前窗口中自身的window对象或当前JavaScript解析器的global对象来访问到。所有的全局对象实际上都是global对象中的成员：在上面的例子中，你可以通过window.fruit、global.fruit或window.animal、global.animal来引用变量fruit和animal。global对象对所有的作用域链和执行上下文都可用。在我们这个只是全局代码的例子里，global对象是这个作用域链中仅有的一个对象。

好吧，这使得函数变得更加不易理解了。除了global对象之外，一个函数的作用域链还包含拥有其自身执行上下文环境的变量对象。

    var fruit = 'banana';
    var animal = 'cat';
    
    function sayFruit() {
        var fruit = 'apple';
    
        console.log(fruit); // 'apple'
        console.log(animal); // 'cat'
    }
    
    console.log(fruit); // 'banana'
    console.log(animal); // 'cat'
    
    sayFruit();

对于全局执行上下文中的代码，fruit和animal标识符分别指向“banana”和“cat”值，因为它们的引用是被存储在执行上下文的variable对象中（也就是global对象中）的。不过，在sayFruit函数里标识符fruit对应的却是另一个值——“apple”。因为在这个函数内部，声明并初始化了另一个变量fruit。因为当前执行上下文中的variable对象在作用域链中处在更靠前的位置（相比全局执行上下文中的variable对象而言），所以JavaScript解析器会知道现在处理的应该是一个局部变量而非全局变量。

因为JavaScript是基于词法作用域的，所以标识符解析还依赖于函数在代码中的位置。一个嵌在函数中的函数，可以访问到其外层函数中的变量：

    var fruit = 'banana';
    
    function outer() {
        var fruit = 'orange';
    
        function inner() {
            console.log(fruit); // 'orange'
        }
    
        inner();
    }
    
    outer();

inner函数中的变量fruit具有一个“orange”的值是因为这个函数的作用域链不单单包含了它自己的variable对象，同时还包含了它被声明时所处的那个函数（这里指outer函数）的variable对象。当解析器遇到inner函数中的标识符fruit，它首先会在作用域链最前面的inner函数的variable对象中寻找与之同名的标识符，如果没有，则去下一个variable对象（outer函数的）中去找。当解析器找到了它需要的标识符，它就会停在那并把fruit的值设置为“orange”。

不过要注意的是，这种方式只适用于采用函数字面量创建的函数。而采用构造函数方式创建的函数则不会这样：

    var fruit = 'banana';
    
    function outer() {
        var fruit = 'orange';
    
        var inner = new Function('console.log(fruit);');
    
        inner(); // 'banana'
    }
    
    outer();

在这个例子里，我们的inner函数不能访问outer函数里的局部变量fruit，所以log语句的输出结果是“banana”而非“orange”。发生这种情况的原因是因为采用new Function()创建的函数其作用域链仅含有它自己的variable对象和global对象，而其外围函数的variable对象都不会被加入到它的作用域链中。因为在这个采用构造函数方式新建的函数自身的variable对象中没有找到标识符fruit，于是解析器去后面一层的global对象中查找，在这里面找到了一个fruit标识符，其值为“banana”，于是被log了出来。

作用域链的创建发生在解析器创建执行上下文之后、变量初始化之前。在全局代码中，解析器首先会创建一个全局执行上下文，之后创建作用域链，之后继续创建全局执行上下文的variable对象（这个对象同时也成为global对象），再之后解析器会进行变量初始化，之后把储存了这些初始化了的变量的variable对象加入到前面创建的作用域链中。在函数代码中，发生的情况也是一样的，唯一不同的是global对象会首先被加入到函数的作用域链，之后把其外围函数的的variable对象加入作用域链，最后加入作用域链的是该函数自己的variable对象。因为作用域链在技术角度来讲属于逻辑上的一个栈，所以解析器的查找操作所遵循的是从栈上第一个元素开始向下顺序查找。这就是为什么我们绝大部分的局部变量是最后才被加入到作用域链却在解析时最先被找到的原因。

## 闭包（Closures）

JavaScript中函数是一等对象以及函数可以引用到其外围函数的变量使得JavaScript相比其他语言具备了一个非常强大的功能：**闭包(closures)**。虽然增加这个概念会使对JavaScript这部分的学习和理解变得更加困难，但必须承认这个特色使函数的用途变得非常强大。在前面我们已经讨论过了JavaScript函数的内在工作机制，这正好能帮助我们了解闭包是如何工作的，以及我们应该如何在代码中使用闭包。

一般情况下，JavaScript变量的生命周期被限定在声明其的函数内。全局变量在整个程序未结束之前一直存在，局部变量则在函数未结束之前一直存在。当一个函数执行完毕，其内部的局部变量会被JavaScript解析器的垃圾回收机制销毁从而不再是一个变量。当一个内嵌函数保存了其外层函数一个变量的引用，即使外层函数执行完毕，这个引用也继续被保存着。当这种情况发生，我们说创建了一个闭包。

不好理解？让我们看几个例子：

    var fruit = 'banana';
    
    (function() {
        var fruit = 'apple';
        console.log(fruit); // 'apple'
    })();
    
    console.log(fruit); // 'banana'

这里，我们有一个创建了一个fruit变量的自执行函数。在这个函数内部，变量fruit的值是apple。当这个函数执行完毕，值为apple的变量fruit便被销毁。于是只剩下了值为banana的全局变量fruit。此种情况下我们并未创建一个闭包。再看看另一种情况：

    var fruit = 'banana';
    
    (function() {
        var fruit = 'apple';
    
        function inner() {
            console.log(fruit); // 'apple'
        }
    
        inner();
    })();
    
    console.log(fruit); // 'banana'

这段代码和上一个很类似，自执行函数创建了一个fruit变量和一个inner函数。当inner函数被调用时，它引用了外层函数中的变量fruit，于是我们的得到了一个apple而不是banana。不幸的是，对于自执行函数来说，这个inner函数是一个局部对象，所以在自执行函数结束后，inner函数也会被销毁掉。我们还是没创建一个闭包，再来看一个例子：

    var fruit = 'banana';
    var inner;
    
    (function() {
        var fruit = 'apple';
    
        inner = function() {
            console.log(fruit);
        }
    
    })();
    
    console.log(fruit); // 'banana'
    inner(); // 'apple'

现在开始变得有趣了。在全局作用域中我们声明了一个名为inner的变量，在自执行函数中我们把一个log出fruit变量值的函数作为值赋给全局变量inner。正常情况下，当自执行函数结束后，其内部的局部变量fruit应该被销毁，就像我们前面2个例子那样。但是因为在inner函数中依然保持着对局部变量fruit的引用，所以最后我们在调用inner时会log出apple。这时可以说我们创建了一个闭包。

一个闭包会在这种情况下被创建：一个内层函数嵌套在一个外层函数里，这个内层函数被储存在其外层函数作用域之外的作用域的variable对象中，同时还保存着对其外层函数局部变量的引用。虽然外层函数中的这个inner函数不会再被运行，但其对外层函数变量的引用却依然保留着，这是因为在函数内部的作用域链中依然保存着该变量的引用，即使外层的函数此时已经不存在了。

要记住一个函数的作用域链同它的执行上下文是绑定的，同其他那些与执行上下文关联紧密的对象一样，作用域链在函数执行上下文被创建之后创建，并随着函数执行上下文的销毁而销毁。解析器只有在函数被调用时才会创建该函数的执行上下文。在上面的例子中，inner函数是在最后一行代码被执行时调用的，而此时，原匿名函数的执行上下文（连同它的作用域链和variable对象）都已经被销毁了。那么inner函数是如何引用到已经被销毁的保存在局部作用域中的局部变量的呢？

这个问题的答案引出了函数内部对象中一个被称为scope属性(scope property)的对象。所有的JavaScript函数都有其自身的内在scope属性，该对象中储存着用来创建该函数作用域链的那些对象。当解析器要为一个函数创建作用域链，它会去查看scope属性看看哪些项是需要被加进作用域链中的。因为相比执行上下文，scope属性同函数本身的联系更为紧密，所以在函数被彻底销毁之前，它都会一直存在——这样苦于保证不了函数被调用多少次，它都是可用的。

一个在全局作用域中被创建的函数拥有一个包含了global对象的scope对象，所以它的作用域链仅包含了global对象和和它自己的variable对象。一个创建在其他函数中的函数，它的scope对象包含了封装它的那个函数的scope对象中的所有对象和它自己的variable对象。

    function A() {
        function B() {
            function C() {
            }
        }
    }

在这个代码片段中，函数A的scope属性中仅保存了global对象。因为函数嵌套在函数A中，所有函数B的scope属性会继承函数A的scope属性的内容并附加上函数A的variable对象。最后，函数C的scope属性会继承函数B的scope属性中的所有内容。

另外，采用函数对象方式（使用new Function()方法）创建的函数，在它们的scope属性中只有一个项，就是global对象。这意味着它们不能访问其外围函数（如果有的话）的局部变量，也就不能用来创建闭包。

## This 关键字（The “this” Keyword）

上面我们讨论了一些函数的内部机制，最后我们还有一个项目要讨论：this关键字。如果你对其他的面向对象的编程语言有使用经验，你应该会对一些关键字感到熟悉，比如this或者self，用以指代当前的实例。不过在JavaScript中this关键字会便得有些复杂，因为它的值取决于执行上下文和函数的调用者。同时this还是动态的，这意味着它的值可以在程序运行时被更改。

this的值总是一个对象，并且有些一系列规则来明确在当前代码块中哪一个对象会成为this。其中最简单的规则就是，在全局环境中，this指向全局对象。

    var fruit = 'banana';
    
    console.log(fruit); // 'banana'
    console.log(this.fruit); // 'banana'

回忆一下，全局上下文中声明的变量都会成为全局global对象的属性。这里我们会看到this.fruit会正确的指向fruit变量，这向我们展示在这段代码中this关键字是指向global对象的。对于全局上下文中声明的函数，在其函数体中this关键字也是指向global对象的。

    var fruit = 'banana';
    
    function sayFruit() {
        console.log(this.fruit);
    }
    
    sayFruit(); // 'banana'
    
    (function() {
        console.log(this.fruit); // 'banana'
    })();
    
    var tellFruit = new Function('console.log(this.fruit);');
    
    tellFruit(); // 'banana'

对于作为一个对象的属性（或方法）的函数，this关键字指向的是这个对象本身而非global对象：

    var fruit = {
    
        name : 'banana',
    
        say : function() {
            console.log(this.name);
        }
    
    };
    
    fruit.say(); // 'banana'

在第三章我们会深入讨论关于对象的话题，但是现在，我们要关注this.name属性是如何指向fruit对象的name属性的。在本质上，这和前面的例子是一样的：因为上面例子中的函数是global对象的属性，所以函数体内的this关键字会指向global对象。所以对于作为某个对象属性的函数而言，其函数体内的this关键字指向的就是这个对象。

对于嵌套的函数而言，遵循第一条规则：不论它们出现在哪里，它们总是将global对象作为其函数体中this关键字的默认值。

    var fruit = 'banana';
    
    (function() {
        (function() {
            console.log(this.fruit); // 'banana'
        })();
    })();
    
    var object = {
    
        fruit : 'orange',
    
        say : function() {
            var sayFruit =  function() {
                console.log(this.fruit); // 'banana'
            };
            sayFruit();
        }
    
    };
    
    object.say();

这里，我们看到处在两层套嵌的子执行函数中的标识符this.fruit指向的是global对象中的fruit变量。在say函数中有一个内嵌函数的例子中，即使say函数自身的this指向的是object对象，但内嵌的sayFruit函数中的this.fruit指向的还是banana。这意味着外层函数并不会对内嵌函数代码体中this关键字的值产生任何影响。

我在前面提到过this关键字的值是可变的，且在JavaScript中能够对this的值进行改变是很有用的。有两种方法可以应用于更改函数this关键字的值：apply方法和call方法。这两种方法实际上都是应用于无需使用调用操作符()来调用函数，虽然没有了调用操作符，但你还是可以通过apply和call方法给函数传递参数。

apply方法接收2个参数：thisValue被用于指明函数体中this关键字所指向的对象；另一个参数是params，它以数组的形式向函数传递参数。当使用一个无参数或第一个参数为null的apply方法去调用一个函数的时候，那么被调用的函数内部this指向的就会是global对象并且也意味着没有参数传递给它：

    var fruit = 'banana'
    
    var object = {
    
        fruit : 'orange',
    
        say : function() {
            console.log(this.fruit);
        }
    
    };
    
    object.say(); // 'banana'
    object.say.apply(); // 'banana'

如果要将一个函数内部的this关键字指向另一个对象，简单的做法就是使用apply方法并把那个对象的引用作为参数传进去：

    function add() {
        console.log(this.a + this.b);
    }
    
    var a = 12;
    var b = 13;
    
    var values = {
        a : 50,
        b : 23
    };
    
    add.apply(values); // 73

apply方法的第二个参数是以一个数组的形式向被调用的函数传递参数，数组中的项要和被调用函数的形参保持一致。

    function add(a, b) {
        console.log(a); // 20
        console.log(b); // 50
        console.log(a + b); // 70
    }
    
    add.apply(null, [20, 50]);

上面说到的另一个方法call，和apply方法的工作机制是一样的，所不同的是在thisValue参数之后跟着的是自选数量的参数，而不是一个数组：

    function add(a, b) {
        console.log(a); // 20
        console.log(b); // 50
        console.log(a + b); // 70
    }
    
    add.call(null, 20, 50);

## 高级的函数技巧（Advanced Function Techniques）

前面的内容主要是关于我们对函数的基础知识的一些讨论。不过，要想完整的展现出JavaScript函数的魅力，我们还必须能够应用前面学到的这些分散的知识。

在下面的章节中，我们会讨论一些高级的函数技巧，并探索目前所掌握的技能其更广泛的应用范围。我想说，本书不会是JavaScript学习的终点，我们不可能把关于这门语言的所有信息都写出来，而应该是开启你探索之路的一个起点。

### 限制作用域（Limiting Scope）

现在，我在维护一个用户的姓名和年龄这个事情上遇到了问题。

    // user对象保存了一些信息
    var user = {
        name : 'Mark',
        age : 23
    };
    
    function setName(name) {
        // 首先确保name是一个字符串
        if (typeof name === 'string') user.name = name;
    }
    
    function getName() {
       return user.name;
    }
    
    function setAge(age) {
        // 首先确保age是一个数字
        if (typeof age === 'number') user.age = age;
    }
    
    function getAge() {
        return user.age;
    }
    
    // 设置一个新的名字
    setName('Joseph');
    console.log(getName()); // 'Joseph'
    
    // 设置一个新的年龄
    setAge(22);
    console.log(getAge()); // 22

目前为止，一切都正常。setName和setAge函数确保我们要设置的值是正确的类型。但我们要注意到，user变量是出在全局作用域中的，可以在该作用域内的任何地方被访问到，这回导致你可以不适应我们的设置函数也能够设置name和age的值：

    user.name = 22;
    user.age = 'Joseph';
    
    console.log(getName()); // 22
    console.log(getAge()); // Joseph

很明显这样不好，因为我们希望这些值能够保持其数据类型的正确性。

那么我们该怎么做呢？如何你回忆一下，你会记起一个创建在函数内部的变量会成为一个局部变量，在该函数外部是不能被访问到的，另外闭包却可以为一个函数能够保存其外层函数局部变量的引用提供途径。结合这些知识点，我们可以把user变成一个受限制的局部变量，再利用闭包来使得获取、设置等函数可以对其进行操作。

    // 创建一个自执行函数
    // 包围我们的代码使得user变成局部变量
    (function() {
    
        // user对象保存了一些信息
        var user = {
            name : 'Mark',
            age : 23
        };
    
        setName = function(name) {
            // 首先确保name是一个字符串
            if (typeof name === 'string') user.name = name;
        };
    
        getName = function() {
            return user.name;
        };
    
        setAge = function(age) {
            // 首先确保age是一个数字
            if (typeof age === 'number') user.age = age;
        };
    
        getAge = function() {
            return user.age;
        }
    
    })();
    
    // 设置一个新的名字
    setName('Joseph');
    console.log(getName()); // 'Joseph'
    
    // 设置一个新的年龄
    setAge(22);
    console.log(getAge()); // 22

现在，如果有什么人想不通过我们的setName和setAge方法来设置user.name和user.age的值，他就会得到一个报错。

### 柯里化（Currying）

函数作为一等对象最大的好处就是可以在程序运行时创建它们并将之储存在变量里。如下面的这段代码：

    function add(a, b) {
      return a + b;
    }
    
    add(5, 2);
    add(5, 5);
    add(5, 200);

这里我们每次都使用add函数将数字5和其他三个数字进行相加，如果能把数字5内置在函数中而不用每次调用时都作为参数传进去是个不错的主意。我们可以将add函数的内部实现机制变为5 + b的方式，但这会导致我们代码中其他已经使用了旧版add函数的部分发生错误。那有没有什么方法可以实现不修改原有add函数的优化方式？

当然我们可以，这种技术被称为**柯里化(partial application 或 currying)**，其实现涉及到一个可为其提前“提供”一些参数的函数：

    var add= function(a, b) {
        return a + b;
    };
    
    function add5() {
        return add(5, b);
    }
    
    add5(2);
    add5(5);
    add5(200);

现在，我们创建了一个调用add函数并预置了一个参数值（这里是5）的add5函数，add5函数本质上来讲其实就是预置了一个参数（柯里化）的add函数。不过，上面的例子并没展示出这门技术动态的一面，如果我们提供的默认值是另外一个应该怎么做？按照上面的例子，我们必须要再次新建一个函数来提供一个新的预置参数。

函数作为一等对象迟早都会派上用处，看，下面应用场景来了。不同于明确的创建一个新的add5函数，我们可以像下面这样来做。

    function add(a, b) {
        return a + b;
    }
    
    function curryAdd(a) {
        return function(b) {
            return add(a, b);
        }
    }
    
    var add5 = curryAdd(5);
    
    add5(2);
    add5(5);
    add5(200);

现在来介绍一下这个新的函数curryAdd，它接收一个参数，这个参数会作为add函数的参数a，同时返回一个新的匿名函数，这个匿名函数接收一个参数b来作为add函数的另一个参数。当我们通过curryAdd(5)来调用这个函数时，它返回一个已经储存了我们一个明确参数值的函数，这个参数值此时被当做是这个匿名函数的一个局部变量。因为我们创建了一个闭包，所以即使这个匿名函数已经执行完毕，但我们还是可以通过它来最终求出我们需要的a + b的值。

我们这里指介绍了一种柯里化函数一个极为简单常见的应用场景，但这可以很好的说明柯里化函数是如何工作的。了解并掌握这种技巧会对你日常的编程工作带来很多便利的。

### 装饰（Decoration）

另一项综合使用函数动态赋值和闭包的技术被称为**装饰(decoration)**。这里的关键词是“装饰”(decorate)，函数的装饰是指能够动态的为一个函数增加新的功能特性。

现在我们有一个函数，它把一个对象作为参数，它的工作是把这个参数对象内的名值对储存到另一个对象里去：

    (function() {
    
        var storage = {};
    
        store = function(obj) {
            for (var i in obj) storage[i] = obj[i];
        };
    
        retrieve = function(key) {
            return storage[key];
        };
    
    })();
    
    console.log(retrieve('name')); // undefined
    
    store({
        name : 'Mark',
        age : '23'
    });
    console.log(retrieve('name')); // 'Mark'

看起来似乎不错，但如果我们的需求变成不单可以给store函数传由名值对组成的对象做参数，还可以直接传名值对，就是类似store(‘name’, ‘Mark’);这种形式的，那我们目前的函数就不能起作用了，我们需要对函数进行改进。

我们可以通过为store函数套上一层装饰者函数来实现想要的改进：

    var decoratePair = function(fn) {
        return function(key, value) {
            if (typeof key === 'string') {
                var _temp = {};
                _temp[key] = value;
                key = _temp;
            }
            return fn(key);
        }
    };
    
    (function() {
    
        var storage = {};
    
        store = decoratePair(function(obj) {
            for (var i in obj) storage[i] = obj[i];
        });
    
        retrieve = function(key) {
            return storage[key];
        };
    
    })();
    
    console.log(retrieve('name')); // undefined
    
    store('name', 'Mark');
    console.log(retrieve('name')); // 'Mark'

这应该是目前为止我们看过的比较复杂的例子了，让我们一步一步的来分析下这段代码。首先，我们声明了一个名为decoratePair的函数，这个函数只接收一个参数fn，这个函数会被我们进行装饰。之后decoratePair会返回一个新的被装饰过的函数，这个函数接收两个参数，key和value。我们原先的store函数只接收一个对象类型的参数，现在通过装饰者函数可以判断第一个参数是对象还是字符串。如果第一个参数不是字符串，则fn函数会立即被执行；如果第一个参数是字符串，则decoratePair的返回值函数会先把传进去的参数key和value以名值对的方式存进一个私有变量_temp里，之后把_temp赋值给一个变量key，这时变量key引用的是一个符合fn函数参数要求的对象，之后再来调用fn函数。

我们上面的装饰者函数可以确保在调用被包装的fn函数时传输的是类型正确的参数，但是修饰着函数也可以用在函数被调用后为其增加特性。下面有一个简单的装饰者函数，它调用add函数的2个参数，并返回这2个参数的和与第二个参数的积。

    var add = function(a, b) {
        return a + b;
    };
    
    var decorateMultiply = function(fn) {
        return function(a, b) {
            var result = fn(a, b);
            return result * b;
        }
    };
    
    var addThenMultiply = decorateMultiply(add);
    
    console.log(add(2, 3)); // 5
    console.log(addThenMultiply(2, 3)); // 15

装饰者函数的用途很广，它可以帮助你在无需直接修改函数的情况下为其增加功能。它尤其适用于那些你不能直接修改的内建函数和第三方代码。

### 组合（Combination）

**组合(combination)**是一项和装饰者函数相似的技术，它的用途是使用两个（或数个）函数来创造一个新的函数。这和声明一个新的函数不同，组合者函数只是将一个函数的返回值作为参数传给下一个函数。

    **var add = function(a, b) {
        return a + b;
    };
    
    var square = function(a) {
        return a * a;
    };
    
    var result = square(add(3, 5));
    
    console.log(result); // 64**

square(add(3, 5))这段代码显示了组合者函数是如何工作的，但这还不能算一个正确的组合者函数。这里，add(3, 5)的返回值8，作为参数传给了square函数，之后square函数返回了64。要把它变成一个组合者函数，我们要将加工过程自动化，免得每次都要去敲square(add(3, 5))。

    var add = function(a, b) {
        return a + b;
    };
    
    var square = function(a) {
        return a * a;
    };
    
    var combine = function(fnA, fnB) {
        return function() {
            var args = Array.prototype.slice.call(arguments);
            var result = fnA.apply(null, args);
            return fnB.call(null, result);
        }
    };
    
    var addThenSquare = combine(add, square);

    var result = addThenSquare(3, 5);
   
    console.log(result); // 64

在这个代码片段中我们先创建了两个具备单一功能的函数add和square。之后创建了一个组合者函数combine，combine函数接收add和square为参数，在返回的匿名函数里，先将传给匿名调用函数的参数a和b转为一个数组args，之后用apply方法调用add函数，将a与b的和赋值给变量result，最后用call方法调用square方法，计算出最终的结果。

注意在使用组合者函数时，函数的顺序和参数的数量是需要被重点注意的。在我们的例子中，因为square函数只需要一个参数，而add函数需要的则是两个，所以我们不能得到一个squareTheAdd（先乘后加，先传一个参数后传2个参数）函数。因为JavaScript只允许函数返回一个值，所以组合者函数的使用场景往往是被限制在那些只采用单个参数的函数中。

