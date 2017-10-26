---
categories: program
tags: dotnet core sudoku
title: 解数独游戏
date: 2017-10-25 23:30:00 +0800
---
### 解数独游戏
#### 0. 开发环境
* .net Core 2.0
* Visual Studio Code

#### 1. 创建项目
```shell
# 创建解决方案
dotnet new sln
# 创建命令行项目
dotnet new console -o Suduku
# 创建单元测试
dotnet new xunit -o SudukuTest
# 把项目添加到解决方案
dotnet sln add .\Suduku\Suduku.csproj
# 把单元测试添加到解决方案
dotnet sln add .\SudukuTest\SudukuTest.csproj
# 把项目引用添加到单元测试
cd Suduku
dotnet add reference ..\Suduku\Suduku.csproj
```

#### 2. Main
```csharp
Solver solver = new Solver();
solver.Run();
```

#### 3. Solver 成员
##### 说明
用于计算数独问题的类。

类中使用二维数组存储数独矩阵，并将其按区块转制成数独区块矩阵，数独区块矩阵中的每一行对应数独矩阵的一个区块。

在不断的遍历矩阵元素，并计算的过程中，有可能出现以下情况：

1. 填充的数值正确，但剩余的元素的可能数值中不存在唯一性
1. 填充的元素中存在错误，或因为矩阵中存在毫无可能性的元素
1. 数据填充完毕

当第一种情况发生时，从矩阵元素中选取可能的值最少的元素（比如可能的值数量为n），将前n-1种可能性推入分支栈，并继续以第n种可能性继续计算。

当第二种情况发生时，说明最近一次选择的可能性是错误的，那么从分支栈中取出另一种可能性继续计算。

当第三种情况发生时，表示已经得到了正确答案。

##### 变量
<table>
<tbody>
<tr><td>_matrix</td><td>数独矩阵</td></tr>
<tr><td>_blocks</td><td>按区块重新组织的矩阵</td></tr>
<tr><td>_previou</td><td>上一次运算的结果</td></tr>
<tr><td>_loopCounter</td><td>计算次数计数器</td></tr>
<tr><td>_branchs</td><td>可能性分支</td></tr>
</tbody>
</table>

##### 方法
<table>
<tbody>
<tr><td>internal</td><td>void</td><td>Run</td><td>运行主方法</td></tr>
<tr><td>private</td><td>void</td><td>InputMatrix</td><td>输入数独矩阵</td></tr>
<tr><td>private</td><td>void</td><td>SolveMatrix</td><td>尝试解决矩阵</td></tr>
<tr><td>private</td><td>bool</td><td>IsWrong</td><td>判断矩阵的数值是否正确</td></tr>
<tr><td>private</td><td>bool</td><td>CannotContinue</td><td>判断矩阵是否还能继续解算</td></tr>
<tr><td>private</td><td>bool</td><td>Finished</td><td>判断矩阵是否所有元素都赋值了</td></tr>
<tr><td>private</td><td>void</td><td>PushBranches</td><td>创建可能的分支</td></tr>
<tr><td>private</td><td>void</td><td>PrintMatrix</td><td>打印矩阵</td></tr>
<tr><td>private</td><td>Tuple</td><td>CloneFrom</td><td>复制矩阵</td></tr>
<tr><td>private</td><td>void</td><td>ComputePossible</td><td>计算每个元素可能的值</td></tr>
<tr><td>private</td><td>void</td><td>FillNumber</td><td>给可确定值的元素赋值</td></tr>
<tr><td>private</td><td>void</td><td>ComputeRow</td><td>按行计算可能的值</td></tr>
<tr><td>private</td><td>void</td><td>ComputeColumn</td><td>按列计算可能的值</td></tr>
<tr><td>private</td><td>void</td><td>ComputeBlock</td><td>按区块计算可能的值</td></tr>
<tr><td>private</td><td>void</td><td>FillCell</td><td>遍历所有元素，如果元素可能的值只有一个，则对该元素赋值</td></tr>
<tr><td>private</td><td>void</td><td>FillRow</td><td>按行遍历所有元素，如果某元素可能的值在这一行的可能的值中唯一，则将该值赋给该元素</td></tr>
<tr><td>private</td><td>void</td><td>FillColumn</td><td>按列遍历所有元素，如果某元素可能的值在这一列的可能的值中唯一，则将该值赋给该元素</td></tr>
<tr><td>private</td><td>void</td><td>FillBlock</td><td>按区块遍历所有元素，如果某元素可能的值在这一区块的可能的值中唯一，则将该值赋给该元素</td></tr>
<tr><td>private</td><td>void</td><td>ResetCounter</td><td>重置FillRow、FillColumn、FillBlock方法内所用的计数器</td></tr>
<tr><td>private</td><td>bool</td><td>IsRowWrong</td><td>按行检查矩阵是否有错误</td></tr>
<tr><td>private</td><td>bool</td><td>IsColumnWrong</td><td>按列检查矩阵是否有错误</td></tr>
<tr><td>private</td><td>bool</td><td>IsBlockWrong</td><td>按区块检查矩阵是否有错误</td></tr>
<tr><td>private</td><td>void</td><td>FillMatrix</td><td>根据输入的字符串和行索引填充矩阵</td></tr>
</tbody>
</table>

#### 4. 源代码
[戳这里](https://github.com/Hawkeyes0/Sudoku)