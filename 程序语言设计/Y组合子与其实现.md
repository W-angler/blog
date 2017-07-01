�����ݹ��Ҷ�֪����������һ��������������ú�����������������lambda�����У���Ϊ�����������ԣ�Ϊ�˱�ʾ�ݹ飬��Ҫ�õ�һ����trick�Ķ�������Y����ӡ�

### lambda����

�������ҵ���һƪ����[lambda���ʽ��JVM����ʵ�ֵ�һЩ˼��](https://www.w-angler.com/blog/3/2)�У��ᵽ��lamdba�����lambda���ʽ���Լ�����֮��Ĺ�ϵ������Ͳ���׸���ˡ�

�����ο����ϣ�

 * ά���ٿƣ�[������](https://zh.wikipedia.org/wiki/%CE%9B%E6%BC%94%E7%AE%97)
 * [���ж�����¶���ͼ�顪������Ľ�ɫ�Խ���](http://mindhacks.cn/2006/10/15/cantor-godel-turing-an-eternal-golden-diagonal/)
 * [lambda����](http://cgnail.github.io/tags/#lambda%E6%BC%94%E7%AE%97)

### Y�����

�����ڽ�Y�����֮ǰ���Ƚ���һ��`������`�ĸ��

��������ѧ�У�����֪�������һ������`f(x)`�Ľ�����Ա���x��ȣ���`f(x)=x`����ô�����Ϊ�����Ĳ����㡣

�������Ƶأ����ĳ���߽׺�������һ��������Ϊ����������ֵҲΪ�����������ô���ǿ��Գ��������Ϊ�߽׺����Ĳ����㡣���磬`F(x)`����һ������`h(x)`��������`h(x)`����`F(h(x))=h(x)`����ô`h(x)`����������Ǹ߽׺���`F(x)`��`������`��

���������ʲô�ã�

��������Ҫ֪�����ǣ���lambda�����У���û��`������`������Ҳ��˵��������������������һ��������Haskell���壩��

    f x=if x>1 then x*(f x-1) else 1

������Haskell��Lisp���ֺ���ʽ�����У��������ǿ�����һ����Ӧ�����Ƶģ�������lambda�����У�ȴ������������Ϊ�������������ģ��޷��ں������ڵ����Լ�����

�������ǣ������������ó��ˡ�����������Ҫ����һ����`F`��ֻ��һ�����ʣ�����Ȼ�������������������ݹ麯����ͬʱ��`f`��`F`�Ĳ�����Ļ�����ô��������ǿ���������f����`F(f)=f`��Ҳ����`f=F(f)=F(F(f))=...`�Ļ�����ô�������ԣ����ǾͿ��Ա�ʾ�����ĵݹ��ˡ�

������`Y�����`�����ã����Ǽ���������Ĳ���������ӣ��Ӷ�ʵ�ֺ����ĵݹ顣Y���������[Haskell B. Curry](https://zh.wikipedia.org/wiki/%E5%93%88%E6%96%AF%E5%87%B1%E7%88%BE%C2%B7%E5%8A%A0%E9%87%8C)���ֵġ�

����Y����ӵ��������£�

    ��f.(��x.(f (x x)) ��x.(f (x x)))

�����������������һͷ��ˮ��û�£���������һ�����ԡ�

����Ϊ�˿������������һЩ������������Y���������Y����ӡ�����

    Y=��f.(��x.(f (x x)) ��x.(f (x x))) 

������һ������hΪ�������ǿ��Կ������ͨ��Y��������һ������������ӵģ�

     (Y h)
    =(��f.(��x.(f (x x)) ��x.(f (x x))) h)
    =(��x.(h (x x)) ��x.(h (x x)))
    =(��y.(h (y y)) ��x.(h (x x)))
    =(h (��x.(h (x x)) ��x.(h (x x))))
    =(h (Y h))

�������Կ�����(Y h)����h�Ĳ���������ӡ�

### Y����ӵ�ʵ��

�����������֪��Y�������lambda�����Ƿǳ���Ҫ�ģ�����һ��֤����lambda������ͼ����ĵȼۣ�����˵������ͼ������Ա�ʾ�ݹ飬������ϵͳ�ı�ʾ���ǵȼ۵ģ���

����������ʵ�ʵı���У���ʹ�Ǻ���ʽ���ԣ�Ҳ����Ҫ��Y����ӣ���Ϊ���ǵļ�����ǽ�����ͼ���֮�ϵģ�����ʽ���Ե�ʵ��������ͼ���������lambda���㡣�����һ�����Կ���lambda�����ͼ����ĵȼۣ������Ƕ��ṩ�˷������ĵݹ麯��֧�֡�

�����������ǿ���ͨ�����ʵ��Y������������������⡣

��������������һЩʵ�֣�

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

�����ðɣ�����ʹ��java ��lambda���ʽ��д������д���ˡ�

****

�����Ҷ�������Լ�������������10��������Javaʵ���ˣ�

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

�������ò�˵��Java��lambdaʵ��̫�ӣ���Ȼ����Ҫ����ת������
