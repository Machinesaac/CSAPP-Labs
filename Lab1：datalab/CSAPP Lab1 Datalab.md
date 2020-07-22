# CSAPP Lab1: Datalab

## 题解

### 1.bitXor

 * *bitXor - **x^y using only ~ and &*** 

 * *Example: bitXor(4, 5) = 1*

 * *Legal ops: ~ &*

 * *Max ops: 14*

 * *Rating: 1*

   #### 思路：

   异或的运算法则为: 
   $$
   a⊕b = (¬a ∧ b) ∨ (a ∧¬b)
   $$
   但因为题目要求只允许用 非 和 与 运算，则可运用德摩根律(De Morgan's laws)变形为：
   $$
   a⊕b = ¬(¬(¬a ∧ b) ∧ ¬(a ∧¬b))
   $$
   满足题目要求。

   #### 代码：

   ```c
   int bitXor(int x, int y) {
   	//德摩根律 
     	return ~(~(~x^y)&~(x&~y)); 
   }
   ```

   

   ### 2.tmin

    * t*min - **return minimum two's complement integer*** 

    * *Legal ops: ! ~ & ^ | + << >>*

    * *Max ops: 4*

    * *Rating: 1*

      #### 思路：

      简单的补码表示。

      #### 代码：

      ```c
      int tmin(void) {
      	//送分题 	
        	return 0x80000000; 
      }
      ```

      

### 3.isTmax

* *isTmax - **returns 1 if x is the maximum, two's complement number, and 0 otherwise*** 

 * *Legal ops: ! ~ & ^ | +*

 * *Max ops: 10*

 * *Rating: 1*

   #### 思路：

   首先得出32位字长下的最大补码表示：**0x7FFFFFFF**，接着简单地异或判断x是否等于这个最大值。

   #### 代码：

   ```c
   int isTmax(int x) {
   	//简单xor 
   	int s = 0x7FFFFFFF;
     	return !(s^x);
   }
   ```

### 4.allOddBits

* *allOddBits - **return 1 if all odd-numbered bits in word set to 1***

 * *where bits are numbered from 0 (least significant) to 31 (most significant)*

 * *Examples allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1*

 * *Legal ops: ! ~ & ^ | + << >>*

 * *Max ops: 12*

 * *Rating: 2*

   #### 思路：

   要解出这道题，首先要明确，我们只关心奇数位的情况，偶数位是0是1与题目无关。

   则为了防止偶数位干扰我们判断，我们运用掩码(mask)将偶数位全部置0，掩码为：**0xAAAAAAAA**。接着，再将偶位置0后的数与**0xAAAAAAAA**比较（唯一一种成立情况），相同则返回1。

   #### 代码：

   ```c
   int allOddBits(int x) {
   	//先把偶数位全部设为0（防止其干扰）后，与0xAAAAAAAA异或（判断是否一致，一致则return 1） 
   	int m = 0xAAAAAAAA;
   	int tmp;
   	tmp = x & m; 
     return !(tmp^x);
   }
   ```

   

### 5.negate

* *negate - **return -x*** 

 * *Example: negate(1) = -1.*

 * *Legal ops: ! ~ & ^ | + << >>*

 * *Max ops: 5*

 * *Rating: 2*

   #### 思路：

   补码公式:  -x = \~x + 1

   #### 代码：

   ```c
   int negate(int x) {
   	//简单公式 
     	return ~x+1;
   }
   ```

   

### 6.isAsciiDigit

 * *isAsciiDigit - **return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')***

 * *Example: isAsciiDigit(0x35) = 1.*

 * *isAsciiDigit(0x3a) = 0.*

 * *isAsciiDigit(0x05) = 0.*

 * *Legal ops: ! ~ & ^ | + << >>*

 * *Max ops: 15*

 * *Rating: 3*

   #### 思路：

   这题很有意思。我把x的前八位分成两半,高四位(必为0011),低四位(0000~1001),难点在于低四位的真值判断。我是通过画卡诺图的方法得出真值表达式Y （A为最高位3,D为最低位0）。可看出有两种情况，均可满足真值条件，分别令它们为条件a和条件b，满足其中一个条件就成立。即, a. x的第四位为0 或 b. x的第4、3、2位为100  时成立。加上之前的高四位限定，得完整条件。

   <img src="C:\Users\Administrator\Desktop\3db6b31b5149d0331aff0c740c15768.jpg" alt="3db6b31b5149d0331aff0c740c15768" style="zoom: 25%;" />

   #### 代码：

   ```c
   int isAsciiDigit(int x) {
   	//画卡诺图 
   	int a = x & 11111000; //取出判断完整条件a是否成立的位
   	int b = x & 11111110; //取出判断完整条件b是否成立的位
   	return !(a^11111000) | !(b^00111000); //判断是否满足完整条件a或完整条件b
   }
   ```

   

### 7.conditional

* *conditional - **same as x ? y : z*** 

 * *Example: conditional(2,4,5) = 4*

 * *Legal ops: ! ~ & ^ | + << >>*

 * *Max ops: 16*

 * *Rating: 3*

   #### 思路:

   !!x 使 x变为0或1，逻辑值与原来相同；

   若x原逻辑值为1，!!x-1 = 0x0000,若为0,!!x - 1 = 0xFFFF

   则对tmp按位取反后，按下式得出答案

   #### 代码：

   ```c
   int conditional(int x, int y, int z) {
   	int tmp = !!x + (~1+1);
     	return ((~tmp)& y) | (tmp & z);
     }
   ```

   

### 8.isLessOrEqual

* *isLessOrEqual - **if x <= y  then return 1, else return** **0*** 

 * *Example: isLessOrEqual(4,5) = 1.*

 * *Legal ops: ! ~ & ^ | + << >>*

 * *Max ops: 24*

 * *Rating: 3*

   #### 思路：

   我分了三种情况:

   1.符号相同:直接通过符号比较大小

   2.符号不同:通过x与y之差的符号判断大小
   
   3.x=y:单独拿出考虑
   
   #### 代码:
   
```c
   int isLessOrEqual(int x, int y) {
   	int xSign = (x >> 31) & 0x1;//取符号位
   	int ySign = (y >> 31) & 0x1;//同上
   	int eq = !(xSign ^ ySign);//判断符号是否相同
       //三种情况如下
   	int a = eq & !!((x + (~y+1)) >> 31);  //符号相同 ,看差的符号 
   	int b = !eq & (xSign | 0); //符号不同,看 x的符号 
   	int c = ! ((x + (~y+1)) ^ 0) ;// 相等 
     	return a | b | c;
   }
   ```
   
   

### 9.logicalNeg

* *logicalNeg - **implement the ! operator, using all of the legal operators except !*** 

 * *Examples: logicalNeg(3) = 0, logicalNeg(0) = 1*

 * *Legal ops: ~ & ^ | + << >>*

 * *Max ops: 12*

 * *Rating: 4* 

   #### 思路：

   这道题当时卡了很久,一直在想非0数相对于0有什么共性。。。。但其实非常简单。x不为0，则x与-x的最高位必不相同，即最高位或运算恒为1，若x为0则恒为0。注意程序里的右移是算术右移，最高位如果为1，右移31位后结果为0xFFFFFFFF，若为0则为0。

   #### 代码：

   ```c
   int logicalNeg(int x) {
   	return ((i|(~i+1))>>31) + 1;
   }
   ```

   

### 10.howManyBits

### 11.floatScale2

* *floatScale2 - **Return bit-level equivalent of expression 2\*f for floating point argument f.***

 * *Both the argument and result are passed as unsigned int's, but*

 * *they are to be interpreted as the bit-level representation of*

 * *single-precision floating point values.*

 * *When argument is NaN, return argument*

 * *Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while*

 * *Max ops: 30*

 * *Rating: 4*

   #### 思路：

   进入浮点数环节。分三种情况，**非规格化**、**规格化**和**NaN或无穷大**。

   **非规格化**：阶码为0，乘2只需将尾码左移一位，并保持符号不变。

   **规格化**：直接将阶码加一。这里理解不了的话可以自己拿个浮点数算一下。

   **NaN或无穷**：返回本身。

   #### 代码：

   ```c
   unsigned floatScale2(unsigned uf) {
     unsigned tmp = uf;
     if((tmp&0x7f800000)==0){   //非规格情况
       	tmp = (tmp&0x80000000)|((tmp&0x007fffff)<<1); //保留符号位
     }
     else if((tmp&0x7f800000)!=0x7f800000){ //判断是否为NaN或无穷
       	tmp += (1<<23); //将阶码加一
       	if((tmp&0x7f800000)==0x7f800000){ //判断是否变为NaN或无穷(阶码全为1)
         		tmp = (tmp>>23)<<23;
       }
     }
     return tmp;
   }
   ```

   （代码来自 @马天猫Masn ，自加注释）

### 12.floatFloat2Int

* *floatFloat2Int - **Return bit-level equivalent of expression (int) f for floating point argument f.***

 * *Argument is passed as unsigned int, but*

 * *it is to be interpreted as the bit-level representation of a*

 * *single-precision floating point value.*

 * *Anything out of range (including NaN and infinity) should return*

 * *0x80000000u.*

 * *Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while*

 * *Max ops: 30*

 * *Rating: 4*

   #### 思路：取出符号位，阶码,尾数后转换，需注意溢出

   #### 代码：(参考 https://zhuanlan.zhihu.com/p/59534845?utm_source=qq)

   ```c
   int floatFloat2Int(unsigned uf) {
   	int s_    = uf>>31; //取符号位 
     	int exp_  = ((uf&0x7f800000)>>23)-127; //取阶码 
     	int frac_ = (uf&0x007fffff)|0x00800000; //取尾数,并补上省略的小数点前一位 
     	if(!(uf&0x7fffffff)) return 0;
   
     	if(exp_ > 31) return 0x80000000; //最大int是pow(2,31), 指数超过则返回溢出 
     	if(exp_ < 0) return 0; //若指数小于0则 uf < 1, 返回0 
   
     	if(exp_ > 23) frac_ <<= (exp_-23); //若指数大于23，则直接将取出的尾数左移多出的位（取出的尾数已经默认左移了23位） 
     	else frac_ >>= (23-exp_); //若指数小于23， 则右移回之前默认多移的位数。 
   
     	if(!((frac_>>31)^s_)) return frac_; //若转换后符号与原来相同，则直接返回 
     	else if(frac_>>31) return 0x80000000;
     	else return ~frac_+1;
   }
   ```

   

### 13.floatPower2

* *floatPower2 - **Return bit-level equivalent of the expression 2.0^x***

 * *(2.0 raised to the power x) for any 32-bit integer x.*
    
***
    
 * *The unsigned value that is returned should have the identical bit*

 * *representation as the single-precision floating-point number 2.0^x.*

 * *If the result is too small to be represented as a denorm, return*

 * 0. *If too large, return +INF.*

 * 

 * *Legal ops: Any integer/unsigned operations incl. ||, &&. Also if, while* 

 * *Max ops: 30* 

 * *Rating: 4*

   #### 思路：

   **2**的IEEE浮点数表示为**0x40000000**，尾数全为0。e = E - bias ,即 E = x - bias = x +127。又E的取值范围为  0<=E<=255, 则E小于0返回0，大于0 return INF。

   #### 代码：

   ```c
   float floatPower2(int x) {
   	int INF = 0xff<<23;
     	int E = x + 127;
     	if(E <= 0) return 0;
     	if(E >= 255) return INF;
     	return E << 23;
   }
   ```

   

