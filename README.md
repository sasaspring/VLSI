# VLSI
Knowledge for VLSI

verilog
一、基本语法
1行为级建模
RTL级也是行为级，模块内部只包含过程块和连续赋值语句（assign），不包含模块实例和基本元件实例。
1.1过程块
initial（仿真）和always（综合，可以由事件触发）过程块，其由过程性赋值语句（过程赋值语句和过程连续赋值语句）和高级程序语句（条件分支语句和循环控制语句）构成。每个过程块都是独立进程，仿真都从0时刻开始并行进行，过程块不能嵌套，其内部多条语句可以顺序执行（begin-end）或并行执行(fork-join)
1.2时间控制
1.2.1延时控制
使用延时表达式控制语句的执行时间。
#10 a=b; //外部时间控制（常规延迟控制），#10执行完了再执行a=b
a= #10 b;//内部时间控制（内嵌赋值延迟控制），先执行temp=b,再延时10，最后a=temp
1.2.2事件控制
边沿触发事件
指定信号变化时刻触发，@(<事件表达式>) 语句；
事件表达式为：a.<信号名> b. posedge<信号名> c. negedge<信号名>
a:信号变化时触发；b:信号上升沿触发（0->x/z/1或x/z/->1）；c:信号下降沿触发（1->x/z/0或x/z->0）；
电平触发事件（wait）
wait (条件表达式) 语句；
条件表达式为真时，执行语句，为电平触发。
1.3赋值语句
过程赋值
阻塞(=)
  首先计算右端表达式的取值，然后立即赋值给左端。
  顺序块中，下一条语句会被本条阻塞，只有本条执行完毕才开始下一条语句执行。
非阻塞(<=)
  本条语句执行完毕之前，下一条语句也可以开始执行。
  首先计算右端表达式的取值，然后等到当前仿真时间结束时再赋值给左端。
连续赋值
  在过程块之外，只能对线网赋值，线网的某一位或某几位。
  隐式，声明时赋值
  显式，先声明，再assign赋值
过程连续赋值
  在过程块内部，可以对寄存器赋值，不能对变量或线网的某一位或某几位赋值。在撤销过程连续赋值前，赋值连续驱动。
assign-deassign
  只能针对寄存器，不能处理线网。优先级高于普通过程赋值语句。
force-release
  可以处理寄存器和线网，优先级高于assign
1.4分支语句
在过程块内部
if-else
  if （expr1）statement1
  else if （expr2）statement2
  else statement3
可以用begin-end块来明确if与else的配对。
  if （expr1）
  begin
    statement1
  end
  else
  begin
    statement2
  end
case
全位比较
case（op_code）
  2'b00：out = a&b；
  2'b01：out = a|b；
  3'b10：out = a^b；
  3'b11：out = a^～b；
  default：out = 0；
endcase
忽略z
casez（op_code）
  4'1zzz：out = a + b；
  4'01zz：out = a - b；
  4'001z：out = a | b；
  4'0001：out = a & b；
  default：out = 0；
endcase
忽略z和x
casex（op_code）
  4'1zxx：out = a + b；
  4'01xz：out = a - b；
  4'001z：out = a | b；
  4'0001：out = a & b；
  default：out = 0；
endcase
1.5循环语句
forever
无限循环执行，可以通过disable终止。

repeat
执行指定次数循环。

while
循环到条件不成立。

for
