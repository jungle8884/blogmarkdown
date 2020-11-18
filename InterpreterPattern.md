---
title: 解释器模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-05-13 21:37:09
author: Jungle

---
# 引言 #
- 解释器这个名词想必大家都不会陌生，比如编译原理中，一个算术表达式通过词法分析器形成词法单元，而后这些词法单元再通过语法分析器构建语法分析树，最终形成一颗抽象的语法分析树。
- 诸如此类的例子也有很多，比如编译器、正则表达式等等。
- 如果一种特定类型的问题发生的频率足够高，那么可能就值得将该问题的各个实例表述为一个简单语言中的句子，这样就可以构建一个解释器，该解释器通过解释这些句子来解决该问题。
- 就比如正则表达式，它就是解释器模型的一种应用，解释器为正则表达式定义了一个文法，如何表示一个特定的正则表达式，以及如何解释这个正则表达式。

# 定义 #
- 给定一个语言, 定义它的文法的一种表示，并定义一个解释器，该解释器使用该表示来解释语言中的句子。
	- 这种模式实现了文法表达式处理的接口，该接口解释一个特定的上下文。
	- 这里提到的文法和句子的概念同编译原理中的描述相同，“文法”指语言的语法规则，而“句子”是语言集中的元素。

# 优缺点 #

## 优点 ##
1. 扩展性好。由于在解释器模式中使用类来表示语言的文法规则，因此可以通过继承等机制来改变或扩展文法。
2. 容易实现。在语法树中的每个表达式节点类都是相似的，所以实现其文法较为容易。

## 缺点 ##
1. 执行效率较低。解释器模式中通常使用大量的循环和递归调用，当要解释的句子较复杂时，其运行速度很慢，且代码的调试过程也比较麻烦。
2. 会引起类膨胀。解释器模式中的每条规则至少需要定义一个类，当包含的文法规则很多时，类的个数将急剧增加，导致系统难以管理与维护。
3. 可应用的场景比较少。在软件开发中，需要定义语言文法的应用实例非常少，所以这种模式很少被使用到。

# 应用场景 #
- 当语言的文法较为简单，且执行效率不是关键问题时；
- 当问题重复出现，且可以用一种简单的语言来进行表达时；
- 当一个语言需要解释执行，并且语言中的句子可以表示为一个抽象语法树时；

# 代码实现 #

## sample代码 ##
----------
		package com.InterpreterPattern.sample;
		
		public class Context {
		    private String input;
		
		    public String getInput(){
		        return input;
		    }
		
		    public void setInput(String input) {
		        this.input = input;
		    }
		
		    private String output;
		
		    public String getOutput() {
		        return output;
		    }
		
		    public void setOutput(String output) {
		        this.output = output;
		    }
		}

----------
		package com.InterpreterPattern.sample;
		
		//抽象表达式声明一个抽象的解释操作,这个接口为抽象语法树中所有的节点所共享.
		public abstract class AbstractExpression {
		    public abstract void Interpret(Context context);
		}

----------
		package com.InterpreterPattern.sample;
		
		public class TerminalExpression extends AbstractExpression {
		    @Override
		    public void Interpret(Context context) {
		        System.out.println("终端解释器");
		    }
		}

----------
		package com.InterpreterPattern.sample;
		
		public class NonTerminalExpression extends AbstractExpression {
		    @Override
		    public void Interpret(Context context) {
		        System.out.println("非终端解释器");
		    }
		}

----------
		package com.InterpreterPattern;
		
		import com.InterpreterPattern.sample.AbstractExpression;
		import com.InterpreterPattern.sample.Context;
		import com.InterpreterPattern.sample.NonTerminalExpression;
		import com.InterpreterPattern.sample.TerminalExpression;
		
		import java.util.ArrayList;
		import java.util.List;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        /*sample测试*/
		        {
		            Context context = new Context();
		            List<AbstractExpression> list = new ArrayList<AbstractExpression>();
		            list.add(new TerminalExpression());
		            list.add(new NonTerminalExpression());
		            list.add(new TerminalExpression());
		            list.add(new TerminalExpression());
		            for (AbstractExpression exp:list
		                 ) {
		                exp.Interpret(context);
		            }
		        }
		    }
		}

----------
**输出**

		终端解释器
		非终端解释器
		终端解释器
		终端解释器
		
		Process finished with exit code 0

----------
## 实例代码---整数算术表达式: 12+6*3-7-2+7/2 ##

		〈算术表达式〉::=〈项〉|〈算术表达式〉+〈项〉|〈算术表达式〉-〈项〉
		〈项〉::=〈常数〉|〈项〉*〈常数〉|〈项〉/〈常数〉
		〈常数〉::=〈数字〉|〈常数〉〈数字〉
		〈数字〉::=0|1|2|3|4|5|6|7|8|9
----------
		package com.InterpreterPattern.example;
		
		public class Context {
		    private String ExpString;
		    private int CurrentPosition=0;
		    private Integer ItemValue= null;    //记录当前项值
		    private Integer ResultValue= null;  //记录当前表达式值
		
		    public void setExpString( String exp) {
		        ExpString=exp;
		    }
		
		    public String getExpString() {
		        return ExpString.substring(CurrentPosition);
		    }
		
		    public void NextPosition(int offset) {
		        CurrentPosition+=offset;
		    }
		
		    public void setItemValue(Integer intVal) {
		        ItemValue=intVal;
		    }
		
		    public Integer getItemVale() {
		        return ItemValue;
		    }
		
		    public void setResultValue(Integer intVal) {
		        ResultValue=intVal;
		    }
		
		    public Integer getResultValue() {
		        return ResultValue;
		    }
		}

----------
		package com.InterpreterPattern.example;
		
		//数字
		public class Digit extends NumericExpression {
		    @Override
		    public Integer interpret(Context context) {
		        if (context.getExpString().length()==0) return null;
		        String c=context.getExpString().substring(0, 1);
		        if (c.compareTo("0")>=0&&c.compareTo("9")<=0) {
		            context.NextPosition(1);
		            return Integer.parseInt(c);
		        } else return null;
		    }
		}

----------

		package com.InterpreterPattern.example;
		
		//算术表达式
		public class NumericExpression {
		    AddExpression addExp;
		    SubExpression subExp;
		    NumericItem NItem;
		    public Integer interpret(Context context) {
		        if (context.getExpString().length()==0)
		            return null;
		
		        Integer iVal;
		        NItem = new NumericItem();//项
		        iVal=NItem.interpret(context);
		        if (iVal==null)
		            return null;
		        context.setResultValue(iVal);
		
		        /*
		         * NItem.interpret(context) 会调用 NextPosition(int offset) 一次
		         * 所以 getExpString() 是 ExpString.substring(CurrentPosition), 即 context.getExpString()为 "当前项值" 后面的表达式
		         * context.getExpString().length()==0 表达的意思是: 后面没有表达式了
		         * */
		        if (context.getExpString().length()==0)
		            return context.getResultValue();
		
		        String c = context.getExpString().substring(0, 1);
		        while (c.equals("+")||c.equals("-")) {
		            context.NextPosition(1);
		            switch (c) {
		                case "+":
		                    addExp = new AddExpression();
		                    addExp.interpret(context);
		                    break;
		                case "-":
		                    subExp = new SubExpression();
		                    subExp.interpret(context);
		                    break;
		            }
		            if (context.getExpString().length()==0)
		                return context.getResultValue();
		
		            c = context.getExpString().substring(0, 1);
		        }
		        return context.getResultValue();
		    }
		}


----------
		package com.InterpreterPattern.example;
		
		//加运算表达式
		public class AddExpression extends NumericExpression {
		    NumericItem NItem;
		    @Override
		    public Integer interpret(Context context) {
		        if (context.getExpString().length() == 0)
		            return null;
		
		        NItem = new NumericItem();//项
		        Integer iVal;
		        iVal = NItem.interpret(context);
		        if (iVal==null)
		            return null;
		        context.setResultValue(context.getResultValue() + iVal);
		
		        return context.getResultValue();
		    }
		}

----------
		package com.InterpreterPattern.example;
		
		//减运算表达式
		public class SubExpression extends NumericExpression {
		    NumericItem NItem = new NumericItem();//项
		    @Override
		    public Integer interpret(Context context) {
		        if (context.getExpString().length()==0)
		            return null;
		
		        Integer iVal;
		        iVal=NItem.interpret(context);
		        if (iVal==null)
		            return null;
		        context.setResultValue(context.getResultValue() - iVal);
		
		        return context.getResultValue();
		    }
		}

----------
		package com.InterpreterPattern.example;
		
		//乘项
		public class MultipleItem extends NumericExpression {
		    NumericValue NVale;//常数
		    @Override
		    public Integer interpret(Context context) {
		        if (context.getExpString().length()==0)
		            return null;
		
		        Integer iVal;
		        NVale = new NumericValue();
		        iVal=NVale.interpret(context);
		        if (iVal==null)
		            return null;
		        context.setItemValue(context.getItemVale()*iVal);
		
		        return context.getItemVale();
		    }
		}

----------
		package com.InterpreterPattern.example;
		
		//除项
		public class DivideItem extends NumericExpression {
		    NumericValue NVale;
		    @Override
		    public Integer interpret(Context context) {
		        if (context.getExpString().length()==0)
		            return null;
		
		        Integer iVal;
		        NVale = new NumericValue();
		        iVal=NVale.interpret(context);
		        if (iVal==null)
		            return null;
		        context.setItemValue(context.getItemVale()/iVal);
		
		        return context.getItemVale();
		    }
		}


----------
		package com.InterpreterPattern.example;
		
		//项
		public class NumericItem extends NumericExpression {
		    MultipleItem MItem;//乘项
		    DivideItem DItem;//除项
		    NumericValue NVale;//常数
		    @Override
		    public Integer interpret(Context context) {
		        if (context.getExpString().length()==0)
		            return null;
		
		        Integer iVal;
		        NVale = new NumericValue();//常数
		
		        iVal=NVale.interpret(context);
		        if (iVal==null)
		            return null;
		        context.setItemValue(iVal);//设置当前项值
		
		        /*
		        * NVale.interpret(context) 会调用 NextPosition(int offset) 一次
		        * 所以 getExpString() 是 ExpString.substring(CurrentPosition), 即 context.getExpString()为 "当前项值" 后面的表达式
		        * context.getExpString().length()==0 表达的意思是: 后面没有表达式了
		        * */
		        if (context.getExpString().length()==0)
		            return context.getItemVale();
		
		        String c = context.getExpString().substring(0, 1);
		        while (c.equals("*")||c.equals("/")) {
		            context.NextPosition(1);
		            switch (c) {
		                case "*":
		                    MItem = new MultipleItem();
		                    MItem.interpret(context);
		                    break;
		                case "/":
		                    DItem = new DivideItem();
		                    DItem.interpret(context);
		                    break;
		            }
		            //同上
		            if (context.getExpString().length()==0)
		                return context.getItemVale();
		            c = context.getExpString().substring(0, 1);
		        }
		
		        return context.getItemVale();
		    }
		}

----------
		package com.InterpreterPattern.example;
		
		//常数
		public class NumericValue extends NumericExpression {
		    Digit digit = new Digit();
		    @Override
		    public Integer interpret( Context context) {
		        if (context.getExpString().length()==0)
		            return null;
		
		        Integer iVal;
		        iVal = digit.interpret(context);
		        if (iVal==null)
		            return null;
		        Integer numericVal = 0;
		
		        while (iVal!=null) {
		            numericVal = numericVal*10 + iVal;
		            iVal = digit.interpret(context);
		        }
		
		        return numericVal;
		    }
		}

----------
		package com.InterpreterPattern.example;
		
		//数字
		public class Digit extends NumericExpression {
		    @Override
		    public Integer interpret(Context context) {
		        if (context.getExpString().length()==0)
		            return null;
		
		        String c = context.getExpString().substring(0, 1);
		        if (c.compareTo("0") >= 0 && c.compareTo("9") <= 0) {
		            context.NextPosition(1);
		            return Integer.parseInt(c);
		        } else
		            return null;
		    }
		}

----------
		package com.InterpreterPattern;
		
		import com.InterpreterPattern.example.Context;
		import com.InterpreterPattern.example.NumericExpression;
		import java.util.ArrayList;
		import java.util.List;
		
		public class Main {
		
		    public static void main(String[] args) {
		        /*example测试*/
		        {
		            Context context = new Context();
		            NumericExpression nExp = new NumericExpression();
		            String expStr="12+6*3-7-7/2";
		            context.setExpString(expStr);
		            int x = nExp.interpret(context);
		            System.out.printf("表达式 %s = %d\n",expStr ,x);
		        }
		    }
		}

----------
**输出**

- 表达式 12+6*3-7-7/2 = 20
- Process finished with exit code 0

----------
**代码参考**

- 课堂代码
- 大话设计模式

----------













