　　递归大家都知道，就是在一个函数体里面调用函数本身。而在无类型lambda演算中，因为函数的匿名性，为了表示递归，需要用到一个很trick的东西——Y组合子。

### lambda演算

　　在我的另一篇博客[lambda表达式及JVM语言实现的一些思考](https://www.w-angler.com/blog/3/2)中，提到过lamdba演算和lambda表达式，以及它们之间的关系。这里就不再赘述了。

　　参考资料：

 * 维基百科：[λ演算](https://zh.wikipedia.org/wiki/%CE%9B%E6%BC%94%E7%AE%97)
 * [康托尔、哥德尔、图灵——永恒的金色对角线](http://mindhacks.cn/2006/10/15/cantor-godel-turing-an-eternal-golden-diagonal/)
 * [lambda演算](http://cgnail.github.io/tags/#lambda%E6%BC%94%E7%AE%97)

### Y组合子

　　在讲Y组合子之前，先解释一下`不动点`的概念。

　　在数学中，我们知道，如果一个函数`f(x)`的结果与自变量x相等，即`f(x)=x`，那么则称它为函数的不动点。

　　类似地，如果某个高阶函数接受一个函数作为参数，返回值也为这个函数，那么我们可以称这个函数为高阶函数的不动点。例如，`F(x)`接受一个函数`h(x)`，返回了`h(x)`，即`F(h(x))=h(x)`，那么`h(x)`这个函数就是高阶函数`F(x)`的`不动点`。

　　这个有什么用？

　　首先要知道的是，在lambda演算中，并没有`函数名`这个概念，也是说，不能像下面这样定义一个函数（Haskell定义）：

    f x=if x>1 then x*(f x-1) else 1

　　像Haskell、Lisp这种函数式语言中，函数都是可以有一个对应的名称的，但是在lambda演算中，却不能这样，因为函数都是匿名的，无法在函数体内调用自己本身。

　　于是，不动点派上用场了。假设我们需要构建一个叫`F`（只是一个代词，它仍然是匿名函数）的匿名递归函数，同时，`f`是`F`的不动点的话，那么，如果我们可以算出这个f，即`F(f)=f`，也就是`f=F(f)=F(F(f))=...`的话，那么，很明显，我们就可以表示匿名的递归了。

　　而`Y组合子`的作用，就是计算出函数的不动点组合子，从而实现函数的递归。Y组合子是由[Haskell B. Curry](https://zh.wikipedia.org/wiki/%E5%93%88%E6%96%AF%E5%87%B1%E7%88%BE%C2%B7%E5%8A%A0%E9%87%8C)发现的。

　　Y组合子的描述如下：

    λf.(λx.(f (x x)) λx.(f (x x)))

　　看到这里你可能一头雾水，没事，我们演算一遍试试。

　　为了看起来容易理解一些，下面我们用Y代替上面的Y组合子。即：

    Y=λf.(λx.(f (x x)) λx.(f (x x))) 

　　以一个函数h为例，我们可以看下如何通过Y组合子求出一个不动点组合子的：

     (Y h)
    =(λf.(λx.(f (x x)) λx.(f (x x))) h)
    =(λx.(h (x x)) λx.(h (x x)))
    =(λy.(h (y y)) λx.(h (x x)))
    =(h (λx.(h (x x)) λx.(h (x x))))
    =(h (Y h))

　　可以看出，(Y h)就是h的不动点组合子。

### Y组合子的实现

　　由上面可知，Y组合子在lambda演算是非常重要的，它进一步证明了lambda演算与图灵机的等价（就是说，它和图灵机可以表示递归，在类型系统的表示上是等价的）。

　　但是在实际的编程中，即使是函数式语言，也不需要用Y组合子（因为我们的计算机是建立在图灵机之上的，函数式语言的实现依靠于图灵机而不是lambda演算。这个进一步可以看出lambda演算和图灵机的等价），他们都提供了非匿名的递归函数支持。

　　但是我们可以通过编程实现Y组合子来加深对它的理解。

　　以下是它的一些实现：

`Python`:

    def Y(f):
        return (lambda h: h(h))(lambda x: f(lambda *args: x(x)(*args)))

`JavaScript`:

    var Y = function(f) {
        return (function(h) {
            return h(h);
        })(function(x) {
            return f(function() {
                return x(x).apply(null, arguments);
            });
        });
    };

`Common-Lisp`:

    (defun Y (f)
        ((lambda (h)
            (funcall h h))
            (lambda (x)
            (funcall f (lambda (&rest args)
                  (apply (funcall x x) args))))))

`Groovy`:

	def Y = {
		h -> ({
			f -> f(f)
		})({
			f -> h{
				x -> f(f)(x)
			}
		})
	}

　　好吧，尝试使用java 的lambda表达式来写，发现写不了。

****

　　我都佩服我自己，行数控制在10行左右用Java实现了！

`Java`:

    public class YCombinator {
    	@FunctionalInterface
    	interface Combinator<E> {
    		public E call(Object... args);
    	}
    	@SuppressWarnings({ "unchecked", "rawtypes" })
    	public static Combinator<Combinator> Y(final Combinator<Combinator> f) {
    		return (Combinator<Combinator>) ((Combinator)(args1->{
    			final Combinator<Combinator> h = (Combinator<Combinator>)args1[0];
    			return h.call(h);
    		})).call((Combinator)(args2->{
    			final Combinator<Combinator> x=(Combinator<Combinator>)args2[0];
    			return f.call((Combinator)(args3->{
    				return x.call(x).call(args3);
    			}));
    		}));
    	}
    	@SuppressWarnings({ "rawtypes", "unchecked" })
    	public static void main(String[] s) {
    		//A simple test
    		Combinator<Combinator> y = Y((Combinator<Combinator>)(args->{
    			final Combinator<Integer> f = (Combinator<Integer>)args[0];
    			return x->{
    				int n = Integer.parseInt(x[0].toString());
    				return n==1?n:n*f.call(n-1);
    			};
    		}));
    		System.out.println(y.call(233));
    	}
    }
    
    interface Combinator<F> extends Function<Combinator<F>, F> {}
    public static <A,B> Function<A,B> Y(Function<Function<A,B>, Function<A,B>> f){
    	Combinator<Function<A,B>> r = w -> f.apply(x -> w.apply(w).apply(x));
    	return r.apply(r);
    }

　　不得不说，Java的lambda实在太坑，居然还需要类型转换……
