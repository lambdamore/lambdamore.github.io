# Lisp能用来做什么


我曾经跟赵余说过，《歌德尔，埃舍尔，巴赫，集异璧之大成》和《计算机程序的构造与解释》同看，就像同时吃花生米和豆腐干一样相得益彰。实际上这其中每一本都让我高潮连连。我一直找不到更合适的比喻，直到看到《华尔街之狼》里 Leonardo怎样贯通了性与毒品。

用一个小例子来一窥堂奥。

这个小例子叫WU谜题，它出现在《GEB》的第一章。这个谜题是“你能产生WU么？”

一开始的时候，我们有一个符号串WI，有下面几个规则：

1，如果一个归你所有的符号串结尾是I，则可以在后面加上一个U。

2，如果有Wx，那么Wxx也归你所有。e.g. WUI=>WUIUI

3，如果符号串中有III，那么可以用U代替III。

4，如果符号串中有UU，那么可以去掉它。

规则就是这些，现在可以试试找出WU了，能够找出来的第一个人请把应用规则的序列方式告诉我，我请他吃饭并分享这其中的况味。

这是一个简单的形式系统，在这个形式系统中，WI被称为”公理“，由它产生出的符号串被称作”定理“。四条规则被称作”推理规则“。由”公理“出发依次利用”推理规则“得到某”定理“的过程被称为"推导”。"推导“这一概念是向”证明“概念看齐的，但比后者要朴素一点。

这种定义明晰的重复性工作最适合用电脑来做。

我的默认语言是scheme。判断一个语言好坏有很多标准，我倾向于选择其中最直观的一个，那就是在描述众多具有普遍意义的问题时，代码总长度最短的是最好的。关于这个直觉我在之前的一篇讨论什么是更好的语言（不只是程序语言）时阐述过。

——talk is cheap， show me the code——

定义“公理”  

```
(define init (list (list 'm 'i)))
```

定义“推理规则”

```
(define (operator lis)

 (define (rule_1 x)

   (define (end lis)

	 (if (null? (cdr lis)) (car lis) (end (cdr lis))))

   (if (eq? (end  x) 'i) (append x (list u))))

  (define (rule_2 x)

    (append x (cdr x)))

   (define (rule_3 x)

     (cond ((> (length x) 3) 

			 (cond ((and (eq? (cadr x) 'i) (eq? (caddr x) 'i) (eq? (cadddr x) 'i)) (append (list 'm 'u) (cddddr x)))

						   (else '())))

			(else '())))

    (define (rule_4 x)

	   (cond ((or (null? x) (null? (cdr x))) x)

			 ((and (eq? (car x) 'u) (eq? (cadr x) 'u)) (rule_4 (cddr x)))

					 (else (cons (car x) (rule_4 (cdr x))))))

	 (define (erase seq)

	   (filter (lambda(x) (not (null? x))) seq))

	 (erase (list (rule_1 lis) (rule_2 lis) (rule_3 lis) (rule_4 lis)))

	 )
```


定义“记忆”

```
(define dictionary init)



(define (in? value lis)

   (or (equal? value (car lis)) (and (not (null? (cdr lis))) (in? value (cdr lis)))))
```


定义“推导”

```
(define (expand seqs)

  (define (erase_repeat lis)

	(cond ((null? lis) '())

			  (else (cons (car lis) (filter (lambda(x) (not (equal? x (car lis)))) (erase_repeat (cdr lis)))))))

    (define (pre-expand lis)

	    (if (null? lis)

			    '()

					    (append (operator (car lis))

									(pre-expand (cdr lis)))))

	  (define result (filter (lambda(x) (not (in? x dictionary))) (erase_repeat (pre-expand seqs))))

	    (begin 

		   (set!  dictionary (append dictionary result))

		      result))
```


检验目标”定理“是否能够被”推导“出来

```
(define object (list 'm 'u))

(define (f a)

	(if (in? object  a) 'yeah (f (expand a))))

(f init)
```


——talk is not cheap，it turns into code—— 

在接受了“公理”和“推理规则”之后，我们可以选择盲目地试一试，在不知不觉中理解这套规则，也可以用scheme来翻译这套规则。两种方式都很必要。在进行前者时会产生一种微妙的心理活动，即跳出这个形式系统，看看我们到底在做什么，系统中暗含了哪些抽象的现象，比如，只有三个字母，每个字符串开头都是M，等等，这种现象在GEB中被称作metathinking。第二种思考方式，相应地，就是thinking within the system， 在scheme优良的抽象风格下，系统显示出了其内部的结构。接下来重点阐述后者。

我们首先定义了“公理”，并将“公理”作为唯一一条语句写入了一个“知识库”，然后尝试将四条“推理规则”应用于“公理”，合理的应用结果作为”一级定理“，一方面被写入“知识库”，另一方面作为"二级定理"的母定理，即继续进行”推理“的材料使用，直到某一级定理包含了目标定理，整个推理过程结束。挺笨的。如果一个人掉入了所谓的”思维定势“，他就变得像这段程序一样笨了。我们来看一下结果：

```
1 ]=> (expand init)



;Value 12: ((m i u) (m i i))



1 ]=> dictionary



;Value 13: ((m i) (m i u) (m i i))



1 ]=> (expand (expand (expand init)))



;Value 14: ((m i u u u) (m i u u i u u) (m i u u i u) (m i u i u i u i u) (m i u u i) (m i u i i u i) (m i u i i i) (m i i i i i i i i) (m u i))



1 ]=> dictionary



;Value 15: ((m i) (m i u) (m i i) (m i u u) (m i u i u) (m i u i) (m i i i i) (m i u u u) (m i u u i u u) (m i u u i u) (m i u i u i u i u) (m i u u i) (m i u i i u i) (m i u i i i) (m i i i i i i i i) (m u i))



1 ]=> (f init)



;Aborting!: out of memory

;GC #97: took:   0.80  (35%) CPU time,   0.80  (36%) real time; free: 2752857

;GC #98: took:   0.70  (88%) CPU time,   0.70  (99%) real time; free: 2752937

```

多么遗憾呢。StackOverFlow。其实这不难理解。如果那么容易就能推导出来，我怎么会许下请你吃饭的诺言呢。

在字符串复杂到一定程度之后，每一条“推理规则”都是合理的，因为每一条“推理规则”合理应用于某一字符串都只产生一条“定理”（这里没有考虑延迟应用），即”n级定理"的数目是“（n-1）级定理”数目的4的（n-1）次方倍。"0级定理“，即“公理”，有1个，则n级定理有4^n个。如果不过滤重复定理的话，在经过n次“推导”后“知识库”里的“定理”数目大概是1/3（4^(n+1)-1)个。

Bomb！

”推导“需要metathinking。我们在摸索的时候，系统的模式会悄无声息地浮现在脑海中。你不仅可以看出这个模式，而且通过检查那些规则，还可以理解这个模式。这展示了人与机器的区别。这个区别在于：机器有可能在做某件事情时不去观察，而人不可能不去观察。能够跳出正在进行的工作并且看一下已经做了些什么，这是智能固有的特点。

我已经不知道自己抄了多少GEB中的句子了，如果可以，我想把它全文打下来，用scheme去解释里面的每一个小故事。




